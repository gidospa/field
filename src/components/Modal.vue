<template>
  <div id="modal-dialog">
    <div class="dialog-overlay"></div>
    
    <div class="modal-container" v-if="type == 'saved-board'">
      <div class="modal-close">
        <div><span @click="$emit('close', {type:'close'})">&times;</span></div>
      </div>

      <div class="modal-header">
        {{ board.timestamp }}
      </div>

      <div>
        <img :src="board.icon"/>
      </div>

      <div class="modal-button">
        <div class="left-button">
          <button class="default-button" @click="$emit('close', {type:'open', index:board.index})">Open</button>
        </div>
        <div class="right-button">
          <button class="default-button" @click="$emit('close', {type:'delete', index:board.index})">Delete</button>
        </div>
      </div>
    </div>


    <div class="modal-container" v-if="type == 'open-new-board'">
      <div class="modal-close">
        <div><span @click="$emit('close', {type:'close'})">&times;</span></div>
      </div>

      <div class="modal-header">
        FORMATION
      </div>

      
      <div class="select-formation">
        <div class="formation-list" id="left-team">
          <div><span :style="{color: board.homeColor}">&#x25CF;&nbsp;</span>HOME</div>
          <div class="formation"
               v-for="(formation, index) in formations"
               :key="'home'+index"
               :class="{selected: selected.home == index}"
               @click="clickFormation('home', index)">
            {{ formation }}
          </div>
        </div>
        <div class="formation-list" id="right-team">
          <div><span :style="{color: board.awayColor}">&#x25CF;&nbsp;</span>AWAY</div>
          <div class="formation"
               v-for="(formation, index) in formations"
               :key="'away'+index"
               :class="{selected: selected.away == index}"
               @click="clickFormation('away', index)">
            {{ formation }}
          </div>
        </div>
      </div>

      <div class="modal-button">
        <div class="center-button">
          <button class="default-button" @click="clickOk">OK</button>
        </div>
      </div>
    </div>

    
    <div class="modal-container" v-if="type == 'show-team-id'">
      <div class="modal-close">
        <div><span @click="$emit('close', {type:'close'})">&times;</span></div>
      </div>

      <div class="modal-header">
        Player DB teams list<br>
        {{this.board.version}}
      </div>
      <br>
      <span class="team-id" v-html="board.teamHtml"></span>
      
      <div class="modal-button">
        <div class="center-button">
          <button class="default-button" @click="$emit('close', {type:'delete'})">
            DELETE PLAYER DB
          </button>
        </div>
      </div>
    </div>

    
    <div class="modal-container" v-if="type == 'clear-localstorage'">
      <div class="modal-close">
        <div><span @click="$emit('close', {type:'close'})">&times;</span></div>
      </div>
      <div class="modal-header">
        Clear browser data<br>
        (localStorage and cookie)
      </div>
      <div class='modal-checkbox'>
        <input type="checkbox" id="all-board-list" name="all-board-list" value="board-list" v-model="checkedItems">
        <label for="all-board-list">Clear all board list</label><br>
        <input type="checkbox" id="all-storage-accesskey" name="all-storage-accesskey" value="accesskey" v-model="checkedItems">
        <label for="all-storage-accesskey">Clear storage access keys</label><br>
      </div>
      <div>Are you sure?</div>
      <div class="modal-button">
        <div class="left-button">
          <button class="default-button" @click="$emit('close', {type:'clear', items:checkedItems})">
            YES
          </button>
        </div>
        <div class="right-button">
          <button class="default-button" @click="$emit('close', {type:'close'})">
            Cancel
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import {FORMATIONS} from '../utils/config.js'

export default {
  props: {
    type: String,
    board: Object
  },
  data() {
    return {
      formations: FORMATIONS,
      selected: {home: 0, away: 0},
      checkedItems: []
    }
  },
  methods: {
    clickFormation(team, index) {
      this.selected[team] = index
    },
    clickOk() {
      let home = this.formations[this.selected.home]
      let away = this.formations[this.selected.away]
      this.$emit('close', {type: 'open', home, away})
    }
  }
}
</script>

<style>
.dialog-overlay {
    z-index: 1;
    display: block;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 120%;
    background-color: rgba(0, 0, 0, 0.6);
}

.modal-container {
  display: block;
  background-color: #f8f8f8;
  margin: 0;
  padding: 0px 0px 10px;
  border: 0px solid #888;
  width: 300px;
  text-align: center;
  z-index: 99;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  border-radius: 4px;
}

.modal-body {
  margin: 20px 0;
}

.modal-button {
  margin: 1rem auto 0rem;
  width: 256px;
}
.left-button {
    text-align: left;
    float: left;
}
.right-button {
    text-align: right;
}
.modal-close {
    padding-top: 8px;
    padding-right: 10px;
    text-align: right;
    font-weight: bold;
    -moz-user-select: none;
    -webkit-user-select: none;
}
.modal-close:hover,
.modal-close:focus {
    color: #000;
    text-decoration: none;
}
.modal-checkbox {
    margin: 1rem auto;
    text-align: left;
    width: 256px;
}

.formation-list {
    display: inline-block;
    width: 45%;
}
.formation {
    display: flex;
    margin: 0.5rem auto;
    border: 1px solid #888;
    border-radius: 0.25rem;
    height: 1.5rem;
    width: 5rem;
    font-size: 1rem;
    justify-content: center;
    align-items: center;
}
.formation:hover {
    border-color: #222;
}
.formation.selected {
    background-color: #eef;
}
.select-formation {
    margin-top: 1em;
}
</style>
