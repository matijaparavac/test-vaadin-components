<template>
  <div class>
    <vaadin-app-layout>
      <vaadin-drawer-toggle slot="navbar"></vaadin-drawer-toggle>
      <img
        slot="navbar"
        src="https://tripod.hr/img/tripodLogo.svg"
        alt="Tripod Logo"
        width="100"
        height="31"
        referrerpolicy="no-referrer"
      />
      <vaadin-tabs
        slot="drawer"
        orientation="vertical"
        theme="minimal"
        style="margin: 0 auto; flex: 1;"
      >
        <vaadin-tab>
          <iron-icon icon="vaadin:home"></iron-icon>Page 1
        </vaadin-tab>
        <vaadin-tab>
          <iron-icon icon="vaadin:list"></iron-icon>Page 2
        </vaadin-tab>
        <vaadin-tab>
          <iron-icon icon="vaadin:options"></iron-icon>Page 3
        </vaadin-tab>
        <vaadin-tab>
          <iron-icon icon="vaadin:question"></iron-icon>Page 4
        </vaadin-tab>
      </vaadin-tabs>

      <div class="content">
        <h3>Page title</h3>
        <div class="form" @keyup.enter="addPerson">
          <vaadin-text-field
            theme="custom-background"
            label="First Name"
            :value="currentPerson.firstName"
            @input="currentPerson.firstName = $event.target.value"
          ></vaadin-text-field>
          <vaadin-text-field
            theme="custom-background"
            label="Last Name"
            :value="currentPerson.lastName"
            @input="currentPerson.lastName = $event.target.value"
          ></vaadin-text-field>
          <vaadin-button @click="addPerson">Add</vaadin-button>
        </div>
        <vaadin-grid :items.prop="people" theme="row-dividers" column-reordering-allowed multi-sort>
          <vaadin-grid-column class="prisutnost" header="Prisustvo"></vaadin-grid-column>
          <vaadin-grid-column path="firstName" header="First name"></vaadin-grid-column>
          <vaadin-grid-column path="lastName" header="Last name"></vaadin-grid-column>
        </vaadin-grid>
      </div>
    </vaadin-app-layout>
  </div>
</template>

<script>
import "@vaadin/vaadin-button";
import "@vaadin/vaadin-grid";
import "@vaadin/vaadin-text-field";
import "@vaadin/vaadin-checkbox";
import "@vaadin/vaadin-icons";
import "@vaadin/vaadin-app-layout";
import "@vaadin/vaadin-app-layout/vaadin-drawer-toggle";
import "@vaadin/vaadin-tabs";
class Person {
  constructor() {
    this.firstName = "";
    this.lastName = "";
    this.prisutnost = true;
  }
}

export default {
  name: "HelloWorld",
  data: function() {
    return {
      people: [],
      currentPerson: new Person()
    };
  },
  methods: {
    addPerson: function() {
      this.people = [...this.people, this.currentPerson];
      this.currentPerson = new Person();
      document.querySelector(".prisutnost").renderer = (
        root,
        grid,
        rowData
      ) => {
        root.innerHTML = `<vaadin-checkbox checked="${rowData.item.prisutnost}" value="${rowData.item.prisutnost}"></vaadin-checkbox>`;
      };
    }
  }
};

const styleElement = document.createElement("dom-module");
styleElement.setAttribute("theme-for", "vaadin-text-field");

styleElement.innerHTML = `<template>
   <style>
      :host([theme="custom-background"]) [part="input-field"] {
        background-color: #E0E0E0;
   
      font-family: var(--lumo-font-family);
     	 font-size: var(--lumo-font-size-xs);
     	 font-weight: 500;
      }
   </style>
 </template>`;

styleElement.register("custom-background");
</script>

<!-- Add "scoped" attribute to limit CSS to this component only         background-color: var(--lumo-contrast-50pct);-->
<style scoped>

.form {
  margin-bottom: 16px;
}
.form * {
  margin-right: 4px;
}

.vaadin-text-field-container [part="input-field"] {
  flex-grow: 0;
  --lumo-primary-color-10pct: hsla(214, 90%, 52%, 0.5);
  background-color: var(--lumo-contrast-20pct);
}

</style>
