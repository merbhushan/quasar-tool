<template>
  <q-layout view="hHh lpR lfr">

    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="left = !left" />
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg">
          </q-avatar>
          Title
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="left" side="left" bordered>
      <!-- drawer content -->
    </q-drawer>

    <q-drawer v-model="right" side="right" bordered>
      <q-bar style="height:50px;" class="bg-primary text-white">
        <div>{{ selectedTool && selectedTool.title || "" }}</div>
        <q-space />
        <q-btn dense flat icon="filter_list" :menu-offset="[10,10]">
          <q-tooltip>Filter</q-tooltip>
            <q-menu transition-show="flip-right" transition-hide="flip-left">
              <q-card style="border-radius: 5">
                <q-card-section>
                  <div class="">Our Changing Planet</div>
                  <div class="">by John Doe</div>
                </q-card-section>
                <q-card-section class="q-pt-none">
                  {{ "testing" }}
                </q-card-section>
              </q-card>
            </q-menu>
          </q-card>
        </q-btn>
        <q-btn v-if="selectedToolLink" @click="openToolBoxUrl()" dense flat icon="eva-external-link-outline">
          <q-tooltip>Open In New</q-tooltip>
        </q-btn>
        <q-btn dense flat @click="closeToolBox()" icon="eva-close-outline">
          <q-tooltip>Close</q-tooltip>
        </q-btn>
      </q-bar>    
    </q-drawer>

    <q-page-container>
      <q-page-sticky v-if="quickToolCount" position="bottom-right" :offset="[18, 18]" class="z-top mobile-hide">
        <q-fab icon="apps" direction="up" color="primary" >
          <q-fab-action v-for="tool in quickTools" :key="tool.id" @click="openToolBox(tool)" :color="tool.color || 'primary'" :icon="tool.icon">
            <q-tooltip :delay="250" anchor="center left" self="center right" :offset="[5, 5]">
              {{ tool.title }}
            </q-tooltip>
          </q-fab-action>
        </q-fab>
      </q-page-sticky>
      <router-view />
    </q-page-container>

    <q-footer elevated class="bg-grey-8 text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg">
          </q-avatar>
          Title
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>

  </q-layout>
</template>

<script>
export default {
  data () {
    return {
      left: false,
      right: false,
      selectedTool: null,
      quickTools: [
        {
          id: 1,
          title: 'Mail',
          icon: 'mail',
          link: 'https://www.google.com'
        },
        {
          id: 2,
          title: 'Add Contact',
          icon: 'person_add',
          openHere: true,
          link: 'https://www.google.com'
        }
      ]
    }
  },
  computed: {
    quickToolCount(){
      let quickTools = this.quickTools && Array.isArray(this.quickTools) ? this.quickTools : [];
      
      return quickTools.length;
    },
    selectedToolLink(){
      return (this.selectedTool && this.selectedTool.link ? this.selectedTool.link : 0 );
    }
  },
  methods: {
    openToolBox(tool){
      this.selectedTool = tool;
      this.right = true;
    },
    closeToolBox(){
      this.selectedTool = null;
      this.right = false;
    },
    openToolBoxUrl(){
      if(this.selectedTool.openHere)
      {
        window.location = this.selectedTool.link;
      }
      window.open(this.selectedTool.link, '_blank');
    }
  }
}
</script>
