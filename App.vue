<template>
  <div class="container">
    <div class="toolbar" style="display: flex">
      <h3>Story creator example</h3>
      <div class="spacer" style="flex: 1"></div>
      <button v-if="!edit" @click="edit = !edit">Edit</button>
      <button v-else @click="edit = !edit">Cancel edits</button>
    </div>

    <draggable
      tag="ul"
      :list="myArray"
      class="list-group"
      @start="drag = true"
      @end="drag = false"
      v-bind="dragOptions"
      handle=".handle"
      :move="checkMove"
    >
      <transition-group type="transition" :name="!drag ? 'flip-list' : null">
        <li
          v-for="element in myArray"
          :key="element.id"
          class="element"
          :class="element.type"
        >
          <div class="item-header">
            <span class="item-title">{{ element.title }}</span>
            <span :disabled="!edit" v-show="edit" class="handle">Move</span>
          </div>
          <div class="item-content">
            <span class="bubble">
              {{ element.name }}
            </span>
          </div>
        </li>
      </transition-group>
    </draggable>

    <footer class="footer">
      <div v-show="edit" class="actions">
        <button class="actions-text" tabindex="-1">+ Add actions</button>
        <div class="actions-list">
          <button class="user-action" @click="onCloseDialog('user')">
            <!-- @click="dialogUser = !dialogUser" -->
            User
          </button>
          <button class="bot-action" @click="onCloseDialog('bot')">
            <!-- @click="dialogBot = !dialogBot" -->
            Bot
          </button>
        </div>
      </div>

      <!-- <button @click="dialog = !dialog">+ Add element</button> -->

      <!-- <VDialog
        v-model="dialogUser"
        bg-transition="fade"
        class="my-dialog"
        @change="log"
      >
        Add intent to the story<br />
        <button
          aria-label="close"
          class="my-dialog__close"
          @click="onCloseDialog('user')"
        >
          &times;
        </button>
      </VDialog>
      <VDialog
        v-model="dialogBot"
        bg-transition="fade"
        class="my-dialog"
        @change="log"
      >
        Add response to the story.<br />
        <button
          aria-label="close"
          class="my-dialog__close"
          @click="onCloseDialog('bot')"
        >
          &times;
        </button>
      </VDialog> -->
    </footer>
  </div>
</template>

<script>
import draggable from "vuedraggable";
// import { VDialog } from "vuetensils/src/components";

export default {
  name: "App",
  components: {
    draggable,
    // VDialog,
  },
  data() {
    return {
      // dialogUser: false,
      // dialogBot: false,
      drag: false,
      myArray: [
        {
          id: 1,
          title: "Intent: Greet",
          name: "Hello",
          type: "user",
        },
        {
          id: 2,
          title: "Response: bot greet",
          name: "Hi, I am proto bot",
          type: "bot",
        },
        {
          id: 3,
          title: "Intent: Ask for help",
          name: "Can you help me finding what I need?",
          type: "user",
        },
        {
          id: 4,
          title: "Response: Confirm",
          name: "Of course, I mean I can try...",
          type: "bot",
        },
        {
          id: 5,
          title: "Intent: unknown ",
          name: "...",
          type: "user",
        },
      ],
      edit: false,
    };
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost",
      };
    },
  },
  methods: {
    onCloseDialog: function (type) {
      type === "bot" ? (this.dialogBot = false) : (this.dialogUser = false);
      const newIndex = new Date().getTime();
      this.myArray.push({
        id: newIndex,
        title: type === "bot" ? "Response: <Name>" : "Intent: <Name>",
        name: type === "bot" ? "Response example" : "Utterance example",
        type: type,
      });
    },
    checkMove: function () {
      // checkMove: function (evt) {
      //   return evt.draggedContext.element.name !== "...";
    },
    log: console.log,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
}

.container {
  flex: 1;
  display: flex;
  flex-direction: column;
  border: 1px solid #ededed;
  margin: 1rem;
  /* padding: 1rem 0; */
  width: 40rem;
  overflow-y: auto;
}

.list-group {
  min-height: 20px;
  overflow-y: auto;
  box-shadow: inset 0 0 0.25rem 0 #dfdfdf;
  border: 1px solid #dfdfdf;
  flex: 1;
}

.element {
  margin: 1rem auto;
  list-style: none;
}

button {
  border: none;
  outline: none;
  background: none;
  border-radius: 0.2rem;
  padding: 0.4rem 0.8rem;
}

button:hover {
  background: #ededed;
}

.handle {
  cursor: move;
  border: none;
  color: #ffffff;
  background: #b3b3b3;
  padding: 0 0.2rem;
  border-radius: 0.4rem;
  /* background: none; */
  /* box-shadow: 0 0 0.2rem 0.2rem #0000002d; */
}
.handle:hover {
  background: #797979;
}

.user {
  text-align: start;
}
.bot {
  text-align: end;
}

.ghost {
  opacity: 0.5;
  /* background: #b3b3b3; */
  /* margin: .4rem; */
}

.flip-list-move {
  transition: transform 0.5s;
}

.bubble {
  border: 1px solid #a2d1f0;
  padding: 0.4rem 0.8rem;
  margin: 0 0.4rem;
  border-radius: 0.4rem;
  background: #ffffff;
  box-shadow: 0 0 0.4rem 0.2rem#00000010;
}

.item-header {
  padding: 0.2rem 1rem;
}

.item-title {
  margin: 0 1rem;
}

.item-content {
  padding: 1rem;
}

.toolbar,
.footer {
  padding: 0.2rem 0.8rem;
}

.fade-enter-active,
.fade-leave-active {
  transition: 0.5s ease opacity;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.slide-up-enter-active,
.slide-up-leave-active {
  transform: translateY(0);
  transition: 0.5s ease opacity, 0.5s ease transform;
}

.slide-up-enter,
.slide-up-leave-to {
  opacity: 0;
  transform: translateY(10px);
}

.my-dialog .vts-dialog {
  background: rgba(0, 0, 0, 0.7);
}

.my-dialog .vts-dialog__content {
  position: relative;
  border-radius: 7px;
  padding: 20px;
  font-family: sans-serif;
  background: #fff;
  transition: 0.3s ease transform;
}

.my-dialog .fade-enter .vts-dialog__content,
.my-dialog .fade-leave-active .vts-dialog__content {
  transform: translateY(20px);
}

.my-dialog__close {
  position: absolute;
  top: 5px;
  right: 5px;
  border: 0;
  padding: 5px;
  background: transparent;
}

.actions {
  display: flex;
}
/* .actions:focus-within .actions-text::after, */
.actions:hover .actions-text::after {
  content: "  ->";
}
/* .actions:focus-within .actions-list, */
.actions:hover .actions-list {
  height: auto;
  width: auto;
  opacity: 1;
}

.actions-list {
  height: 0;
  width: 0;
  opacity: 0;
  display: inline-flex;
}
</style>