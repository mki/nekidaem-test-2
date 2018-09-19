<template>
	<div class="kanban-board">
		<div class="row">
			<h3 class="header orange">On Hold ({{items0.length}})</h3>
			<Container :group-name="'1'" :get-child-payload="getChildPayload0" @drop="onDrop('items0', $event)">
				<Draggable v-for="item in items0" :key="item.id">
					<div class="draggable-item">
						<div class="delete" @click="deleteItem('items0', item)"></div>
						<p><b class="id">id: </b>{{item.id}}</p>
						<p>{{item.header}}</p>
					</div>
				</Draggable>
			</Container>
			<button class="add-another-card" @click="addItem('items0')">Добавить карточку</button>
		</div>
		<div class="row">
			<h3 class="header blue">In Progress ({{items1.length}})</h3>
			<Container :group-name="'1'" :get-child-payload="getChildPayload1" @drop="onDrop('items1', $event)">
				<Draggable v-for="item in items1" :key="item.id">
					<div class="draggable-item">
						<div class="delete" @click="deleteItem('items1', item)"></div>
						<p><b class="id">id: </b>{{item.id}}</p>
						<p>{{item.header}}</p>
					</div>
				</Draggable>
			</Container>
			<button class="add-another-card" @click="addItem('items1')">Добавить карточку</button>
		</div>
		<div class="row">
			<h3 class="header yellow">Needs Review ({{items2.length}})</h3>
			<Container :group-name="'1'" :get-child-payload="getChildPayload2" @drop="onDrop('items2', $event)"> 
				<Draggable v-for="item in items2" :key="item.id">
					<div class="draggable-item">
						<div class="delete" @click="deleteItem('items2', item)"></div>
						<p><b class="id">id: </b>{{item.id}}</p>
						<p>{{item.header}}</p>
					</div>
				</Draggable>
			</Container>
			<button class="add-another-card" @click="addItem('items2')">Добавить карточку</button>
		</div>
		<div class="row">
			<h3 class="header green">Approved ({{items3.length}})</h3>
			<Container :group-name="'1'" :get-child-payload="getChildPayload3" @drop="onDrop('items3', $event)">
				<Draggable v-for="item in items3" :key="item.id">
					<div class="draggable-item">
						<div class="delete" @click="deleteItem('items3', item)"></div>
						<p><b class="id">id: </b>{{item.id}}</p>
						<p>{{item.header}}</p>
					</div>
				</Draggable>
			</Container>
			<button class="add-another-card" @click="addItem('items3')">Добавить карточку</button>
		</div>
	</div>
</template>

<script>
import { Container, Draggable } from "vue-smooth-dnd";
import { applyDrag, loadItems, saveItems } from "./utils";
export default {
	name: "KanbanBoard",
	components: { Container, Draggable },
	data: function() {
		return {
			items0: loadItems('items0'),
			items1: loadItems('items1'),
			items2: loadItems('items2'),
			items3: loadItems('items3')
		};
	},
	methods: {
		onDrop: function(collection, dropResult) {
			this[collection] = applyDrag(this[collection], dropResult);
			saveItems(collection, this[collection]);
		},
		getChildPayload0: function(index) {
			return this.items0[index];
		},
		getChildPayload1: function(index) {
			return this.items1[index];
		},
		getChildPayload2: function(index) {
			return this.items2[index];
		},
		getChildPayload3: function(index) {
			return this.items3[index];
		},
		addItem: function(collection) {
			const newHeader = prompt('Введите заголовок для этой карточки', '');
			if (newHeader) {
				const d = new Date();
				const newID = d.getTime();
				this[collection].push({ id: newID, header: newHeader });
				saveItems(collection, this[collection]);
			}
		},
		deleteItem: function(collection, item) {
			var index = this[collection].map(x => {
				return x.id;
			}).indexOf(item.id);
			this[collection].splice(index, 1);
			saveItems(collection, this[collection]);
		}
	}
};
</script>



