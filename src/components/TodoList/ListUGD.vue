<template>
    <v-main class="list">
        <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

        <v-card>
            <v-card-title>
                <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search"
                    single-line
                    hide-details
                ></v-text-field>
                <v-spacer></v-spacer>
                <v-btn color="success" dark @click="dialog = true">
                    Tambah
                </v-btn>
            </v-card-title>
            <v-data-table :headers="headers" :items="todos" :search="search">
                <template v-slot:[`item.actions`]="{ item }">
                    <v-btn small class="mr-2" @click="editItem(item)">
                        edit
                    </v-btn>
                    <v-btn small @click="deleteItem(item)">
                        delete
                    </v-btn>
                </template>
                <template v-slot:[`item.priority`]="{ item }">
                    <td>
                        <v-card v-if="item.priority == 'Penting'" style="border-color: lightcoral; color: coral; width: fit-content;" outlined>
                            {{ item.priority }}
                        </v-card>
                        <v-card v-else-if="item.priority == 'Biasa'" style="border-color: lightblue; color: blue; width: fit-content;" outlined>
                            {{ item.priority }}
                        </v-card>
                        <v-card v-else outlined style="border-color: lightgreen; color: green; width: fit-content;">
                            {{ item.priority }}
                        </v-card>
                    </td>
                </template>
            </v-data-table>
        </v-card>

        <v-dialog v-model="dialog" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline" v-if="tambah == true" >Form Todo</span>
                        <span class="headline" v-else>Edit Form Todo</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-text-field
                            v-model="formTodo.task"
                            label="Task"
                            required
                        ></v-text-field>

                        <v-select
                            v-model="formTodo.priority"
                            :items="['Penting', 'Biasa', 'Tidak penting']"
                            label="Priority"
                            required
                        ></v-select>

                        <v-textarea
                            v-model="formTodo.note"
                            label="Note"
                            required
                        ></v-textarea>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="cancel"> Cancel</v-btn>
                    <v-btn v-if="tambah == true" color="blue darken-1" text @click="save">Save</v-btn>
                    <v-btn v-else color="blue darken-1" text @click="edit(formTodo)">Save</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
        <v-dialog v-model="dialogdelete" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">Yakin ingin menghapus?</span>
                </v-card-title>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="green darken-1" text @click="cancel">Tidak</v-btn>
                    <v-btn color="red darken-1" text @click="confirmdelete">Ya</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog> 
    </v-main>
</template>
<script>
export default {
    name: "List",
    data() {
        return {
            search: null,
            dialog: false,
            editTodo: null,
            edititem: null,
            tambah: true,
            dialogdelete:false,
            headers: [
                {
                    text: "Task",
                    align: "start",
                    sortable: true,
                    value: "task",
                },
                
                { text: "Priority", value: "priority" },
                { text: "Note", value: "note" },
                { text: "Actions", value:"actions" },
            ],
            todos: [
                {
                    task: "Bernafas",
                    priority: "Penting",
                    note: "huffttt",
                },
                {
                    task: "Nongkrong",
                    priority: "Tidak Penting",
                    note: "bersama tman2",
                },
                {
                    task: "Masak",
                    priority: "Biasa",
                    note: "masak air 500ml",
                },
            ],
            formTodo: {
                task: null,
                priority: null,
                note: null,
            },
        };
    },
    methods: {
        save() {
            this.todos.push(this.formTodo);
            this.resetForm();
            this.dialog = false;
        },
        cancel() {
            this.resetForm();
            this.dialog = false;
            this.edititem = null;
            this.tambah = true;
            this.dialogdelete = false;

        },
        resetForm() {
            this.formTodo = {
                task: null,
                priority: null,
                note: null,
            };
        },
        editItem(item) {
            this.tambah = false;
            this.formTodo = {
                task: item.task,
                priority: item.priority,
                note: item.note,
            };
            this.dialog = true;
            this.edititem = item;
        },
        edit(formTodo){
            this.edititem.task = formTodo.task;
            this.edititem.priority = formTodo.priority;
            this.edititem.note = formTodo.note;
            this.cancel();
        },
        deleteItem(item) {
            this.dialogdelete = true;
            this.edititem = item;
        },
        confirmdelete() {
            this.todos.splice(this.todos.indexOf(this.edititem), 1);
            this.dialogdelete = false;
        },
    },
};
</script>