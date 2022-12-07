<template>
	<TheHeader />
	<main>
		<div class="container">
			<div class="row">
				<div class="wrapper offset-1 col-10">
					<button class="button" @click="openTaskModal">Создать задачу</button>
					<div class="board-list">
						<VBoard
							title="План"
							:tasks="todoTasks"
							code="todo"
							@move="moveTask"
							@edit-task="openEditTaskModal"
						/>
						<VBoard
							title="В работе"
							:tasks="inProgressTasks"
							code="inProgress"
							@edit-task="openEditTaskModal"
							@move="moveTask"
						/>
						<VBoard
							title="Готово"
							:tasks="completedTasks"
							code="completed"
							@edit-task="openEditTaskModal"
							@move="moveTask"
						/>
					</div>
				</div>
			</div>
		</div>
	</main>
	<VModal
		v-if="modal.isActive"
		:data="formData"
		:type="modal.type"
		@close="closeTaskModal"
		@create="createNewTask"
		@update="updateTask"	
	/>
	<TheFooter />
</template>

<script>

import TheHeader from '@/components/TheHeader.vue';
import TheFooter from '@/components/TheFooter.vue';
import VModal from '@/components/VModal.vue';
import VBoard from '@/components/VBoard.vue'

export default {
	name: 'App',
	components: {
		TheHeader, TheFooter, VModal, VBoard
	},
	data() {
		return {
			tasks: [],
			statuses: ['todo', 'inProgress', 'completed', 'removed'],
			formData: {
				description: '',
				priority: 1,
				taskID: -1
			},
			modal: {
				isActive: false,
				type: 'creating'
			}
		}
	},
	computed: {
		todoTasks() {
			return this.tasks.filter(task => task.status == 'todo').sort((a, b) => a.priority - b.priority);
		},
		inProgressTasks() {
			return this.tasks.filter(task => task.status == 'inProgress').sort((a, b) => a.priority - b.priority);
		},
		completedTasks() {
			return this.tasks.filter(task => task.status == 'completed').sort((a, b) => a.priority - b.priority);
		}
	},
	methods: {
		openTaskModal() {
			this.modal.isActive = true;
			this.modal.type = 'creating';
		},
		openEditTaskModal(taskID) {
			this.formData = {...this.tasks.find(task => task.id == taskID), taskID: taskID};
			this.modal.isActive = true;
			this.modal.type = 'editing';
		},
		closeTaskModal() {
			this.modal.isActive = false;
		},
		createNewTask(description, priority) {
			const currentTask = {
				id: this.tasks.length,
				description: description,
				status: 'todo',
				priority: Number(priority),
				createdAt: new Date()
			}

			this.tasks.push(currentTask);
		},
		moveTask(taskID, destination) {
			let currentStatusIndex = this.statuses.indexOf(this.tasks[taskID].status);

			switch (destination) {
				case 'left': {
					currentStatusIndex--;
					break;
				}
				case 'right': {
					currentStatusIndex++;
					break;
				}
				case 'todo': {
					currentStatusIndex = 0;
					break;
				}
				case 'inProgress': {
					currentStatusIndex = 1;
					break;
				}
				case 'completed': {
					currentStatusIndex = 2;
					break;
				}
			}

			this.tasks[taskID].status = this.statuses[currentStatusIndex];
		},
		saveTasks() {
			const jsonTasks = this.tasks.map(task => ({...task, createdAt: task.createdAt.getTime()}));
			localStorage.setItem('kanban-tasks', JSON.stringify(jsonTasks));
		},
		updateTask(description, priority) {
			this.tasks[this.formData.taskID] = {
				...this.tasks[this.formData.taskID],
				description: description,
				priority: Number(priority)
			}
		}
	},
	watch: {
		tasks: {
			handler() {
				this.saveTasks();
			},
			deep: true
		}
	},
	created() {
		if (localStorage.getItem('kanban-tasks')) {
			this.tasks = JSON.parse(localStorage.getItem('kanban-tasks'))
				.map(task => ({...task, createdAt: new Date(task.createdAt)}));
		}
	}
}
</script>
