<template>
    <div class="todo-wrapper">
        <h1>Todo App</h1>
        <div class="input-wrapper">
            <input class ="text-box" placeholder="Add Todo"  type="text" v-model="text" @keyup.enter="add(text)" />
            <button class="add" @click="add(text)">Add</button>
            <input class ="text-box" placeholder="Search Todo"   type="text" v-model="searchText"/>
        </div>
        <ul class="list-wrapper">
            <li class="list" v-for="(item, i) in renderComp" :key="item">
                <div class="list-container">
                    <input type="checkbox" v-model="item.compeleted" />
                    <span v-if="editField !== i" class="task-name" :class="{ 'strike': item.compeleted }">{{ item.text }}</span>
                    <input v-if="editField === i" type="text"  class="input-text" v-model=item.text @keyup.enter="save()"/>
                </div>
                    <div class="button-wrapper">
                        <button v-if="editField !== i" class="edit" @click="edit(i)"> Edit</button>
                        <button  v-if="editField === i" class="save" @click="save()"> Save</button>
                        <button class="delete" @click="del(i)"> Delete</button>
                    </div>

            </li>
        </ul>
    </div>

</template>
<script>
import {ref, computed} from 'vue';
export default {
    setup() {
        const text = ref('');
        const searchText = ref('');
        const items = ref([]);
        const editField = ref(-1)
        const add = (item) => {
            if(item) {
                items.value.push({text:item, compeleted: false});
            text.value = '';
            }

        }
        const del = (i) => {
           items.value =  items.value.filter((e,index) => index !== i )
        }
        const renderComp = computed(() => {
            if(searchText.value) {
                return items.value.filter(e => e.text.includes(searchText.value) )
            } else {
                return items.value
            }
        })
        const edit = (i) => {
            editField.value = i;
        }
        const  save = () => {
            editField.value = -1;
        }
        return {
            text,
            add,
            del,
            edit,
            items,
            renderComp,
            searchText,
            editField,
            save
        }
    },
}
</script>
<style scoped>
.strike {
    text-decoration: line-through;
}
.todo-wrapper {
    display: flex;
    align-items: center;
    flex-direction: column;
    background: tomato;
    color: white;
    min-height: 80vh;
}
 h1 {
    margin: 20px;
 }
 .input-wrapper,.list-wrapper,.list {
    display: flex;
    width: 100%;
    padding: 30px;
    align-items: center;
 }
 .text-box {
    padding: 0 15px;
    height: 2.5rem;
    width: 45%;
    border-radius: 8px;
    border: 0;
    margin: 1rem;
 }
 button {
    width: 80px;
    height: 40px;
    cursor: pointer;
    border-radius: 8px;
    border: 0;
    margin: 0 5px;
 }
 .list-wrapper {
    list-style: none;
    margin: 0;
    flex-direction: column;
 }
  .list {
    padding: 15px;
    justify-content: space-between;
    margin: 0 40px 5px;
    background: gainsboro;
    color: #000;
  }
  .task-name,.input-text {
    margin-left: 5px;
  }
  .list-container {
    width: calc(100% - 250px);
    display: flex;
    align-items: center;
  }
</style>