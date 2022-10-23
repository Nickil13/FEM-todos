<script>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
let id = 0;
export default {
    data() {
        return {
            newTodo: "",
            hideCompleted: false,
            todos: [
                { id: id++, text: "Test1", done: true },
                { id: id++, text: "Test2", done: true },
                { id: id++, text: "Test3", done: true },
            ],
        };
    },
    methods: {
        addTodo() {
            console.log("adding todo");
        },
    },
};
</script>

<template>
    <main>
        <div class="banner"></div>
        <div>
            <!-- <TheWelcome /> -->
            <div class="header">
                <h1>Todo</h1>
                <button>
                    <img
                        src="./assets/images/icon-sun.svg"
                        alt="light mode icon"
                        width="20"
                        height="20"
                    />
                </button>
            </div>
            <form @submit.prevent="addTodo">
                <input
                    class="todo-input"
                    v-model="newTodo"
                    placeholder="Create a new todo..."
                />
            </form>
            <div>
                <ul>
                    <li v-for="todo in todos" :key="todo.id">
                        <input type="checkbox" v-model="todo.done" />
                        <span :class="{ done: todo.done }">{{
                            todo.text
                        }}</span>
                        <button @click="removeTodo(todo)">X</button>
                    </li>
                </ul>
                <div>
                    <p>items left</p>
                    <button>Clear Completed</button>
                </div>
            </div>
            <div>All Active Completed</div>
            <!-- Add dynamic number -->
            <div>Drag and drop to reorder list</div>

            <!-- <div class="attribution">
                Challenge by
                <a
                    href="https://www.frontendmentor.io?ref=challenge"
                    target="_blank"
                    >Frontend Mentor</a
                >. Coded by <a href="#">Your Name Here</a>.
            </div> -->
        </div>
    </main>
</template>

<style scoped>
.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
.banner {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 200px;
    background-repeat: no-repeat;
    background-size: cover;
    background-image: url("./assets/images/bg-mobile-dark.jpg");
}

.todo-input {
    padding: 0.75rem;
    border-radius: 0.25rem;
    outline: none;
    border: none;
    font-family: inherit;
    background-color: var(--clr-dark-desat-blue);
    width: 100%;
    margin-bottom: 20px;
}
.todos {
    background-color: var(--clr-dark-desat-blue);
    border-radius: 0.25rem;
}
.todo {
    background-color: var(--clr-dark-desat-blue);
    border-radius: 0.25rem;
}
</style>
