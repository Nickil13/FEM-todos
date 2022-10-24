<script>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
let id = 0;
export default {
    data() {
        return {
            newTodo: "",
            hideCompleted: false,
            selectedFilter: "all",
            userTheme: "light-theme",
            todos: [
                { id: id++, text: "Test1", done: true },
                { id: id++, text: "Test2", done: false },
                { id: id++, text: "Test3", done: false },
            ],
        };
    },
    methods: {
        addTodo() {
            this.todos.push({ id: id++, text: this.newTodo, done: false });
            this.newTodo = "";
        },
        setFilter(newFilter) {
            this.selectedFilter = newFilter;
        },
        clearCompleted() {
            this.todos = this.todos.filter((todo) => !todo.done);
        },
        toggleCheck(clickedId) {
            this.todos = this.todos.map((todo) => {
                if (todo.id === clickedId) {
                    todo.done = !todo.done;
                    return todo;
                }
                return todo;
            });
        },
        removeTodo(clickedId) {
            this.todos = this.todos.filter((todo) => todo.id !== clickedId);
        },
        toggleTheme() {
            const activeTheme = localStorage.getItem("user-theme");
            if (activeTheme === "light-theme") {
                this.setTheme("dark-theme");
            } else {
                this.setTheme("light-theme");
            }
        },
        getMediaPreference() {
            const hasDarkPreference =
                window.matchMedia("(prefers-color-scheme: dark)") / matches;
            if (hasDarkPreference) {
                return "dark-theme";
            } else {
                return "light-theme";
            }
        },
        setTheme(theme) {
            this.userTheme = theme;
            localStorage.setItem("user-theme", theme);
            document.documentElement.className = theme;
        },
        getTheme() {
            return localStorage.getItem("user-theme");
        },
    },
    mounted() {
        const initUserTheme = this.getTheme() || this.getMediaPreference();
        this.setTheme(initUserTheme);
    },
    computed: {
        filteredTodos() {
            if (this.selectedFilter === "completed") {
                return this.todos.filter((todo) => todo.done);
            } else if (this.selectedFilter === "active") {
                return this.todos.filter((todo) => !todo.done);
            }
            return this.todos;
        },
    },
};
</script>

<template>
    <main>
        <div class="banner"></div>
        <div class="container">
            <div>
                <!-- <TheWelcome /> -->
                <div class="header">
                    <h1>Todo</h1>
                    <button @click="toggleTheme">
                        <img
                            v-if="userTheme === 'dark-theme'"
                            src="./assets/images/icon-sun.svg"
                            alt="light mode icon"
                            width="20"
                            height="20"
                        />
                        <img
                            v-if="userTheme === 'light-theme'"
                            src="./assets/images/icon-moon.svg"
                            alt="dark mode icon"
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

                <ul class="todos">
                    <li v-for="todo in filteredTodos" :key="todo.id">
                        <input
                            class="checkbox"
                            type="checkbox"
                            v-model="todo.done"
                        />
                        <span
                            @click="toggleCheck(todo.id)"
                            class="custom-checkbox"
                            :class="{ active: todo.done }"
                        >
                            <img
                                :class="{ notChecked: !todo.done }"
                                src="./assets/images/icon-check.svg"
                            />
                        </span>
                        <span :class="{ done: todo.done }">{{
                            todo.text
                        }}</span>
                        <button class="close-btn" @click="removeTodo(todo.id)">
                            <img
                                src="./assets/images/icon-cross.svg"
                                width="10"
                                height="10"
                            />
                        </button>
                    </li>
                    <li class="todos-footer">
                        <p>{{ filteredTodos.length }} items left</p>
                        <button @click="clearCompleted">Clear Completed</button>
                    </li>
                </ul>

                <div class="todos-filter">
                    <button
                        @click="setFilter('all')"
                        :class="selectedFilter === 'all' ? 'activeFilter' : ''"
                    >
                        All
                    </button>
                    <button
                        @click="setFilter('active')"
                        :class="
                            selectedFilter === 'active' ? 'activeFilter' : ''
                        "
                    >
                        Active
                    </button>
                    <button
                        @click="setFilter('completed')"
                        :class="
                            selectedFilter === 'completed' ? 'activeFilter' : ''
                        "
                    >
                        Completed
                    </button>
                </div>

                <div class="drag-and-drop">Drag and drop to reorder list</div>

                <!-- <div class="attribution">
                Challenge by
                <a
                    href="https://www.frontendmentor.io?ref=challenge"
                    target="_blank"
                    >Frontend Mentor</a
                >. Coded by <a href="#">Your Name Here</a>.
            </div> -->
            </div>
        </div>
    </main>
</template>

<style scoped>
.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    color: white;
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
.light-theme .banner {
    background-image: url("./assets/images/bg-mobile-light.jpg");
}

@media screen and (min-width: 800px) {
    .dark-theme .banner {
        background-image: url("./assets/images/bg-desktop-dark.jpg");
    }
    .light-theme .banner {
        background-image: url("./assets/images/bg-desktop-light.jpg");
    }
}

.todo-input {
    padding: 1rem;
    border-radius: 0.25rem;
    outline: none;
    border: none;
    font-family: inherit;
    background-color: var(--clr-base);
    box-shadow: var(--shadow);
    width: 100%;
    margin-bottom: 20px;
    color: inherit;
}
.checkbox {
    display: none;
}
.notChecked {
    display: none;
}
.custom-checkbox.active {
    padding: 0.25rem;
    background: var(--clr-primary-bg);
    border-radius: 1rem;
    display: inline-flex;
    margin-right: 10px;
    width: 20px;
    height: 20px;
}
.custom-checkbox {
    padding: 0.25rem;
    border: 1px solid var(--clr-outline);
    border-radius: 1rem;
    display: inline-flex;
    margin-right: 10px;
    width: 20px;
    height: 20px;
    cursor: pointer;
}
.todos {
    /* background-color: var(--clr-dark-desat-blue); */
    background-color: var(--clr-base);
    border-radius: 0.25rem;
    overflow: clip;
    box-shadow: var(--shadow);
}

.todos > li {
    display: flex;
    align-items: center;
    background-color: var(--clr-dark-desat-blue);
    padding: 1rem;
    position: relative;
    color: var(--clr-light-gray-blue);
}
.todos > li .close-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    right: 20px;
    color: var(--clr-very-dark-gray-blue);
}
.todos > li + li {
    border-top: 1px solid var(--clr-outline);
}
.todos > li.todos-footer {
    display: flex;
    justify-content: space-between;
    color: var(--clr-dark-gray-blue);
}
.todos-filter {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: var(--clr-base);
    padding: 1rem 0.75rem;
    margin: 1rem 0;
    border-radius: 0.25rem;
    box-shadow: var(--shadow);
}
.todos-filter button {
    margin: 0 0.5rem;
    font-weight: bold;
}

.drag-and-drop {
    margin: 3rem 0 1rem 0;
    text-align: center;
}

.activeFilter {
    color: var(--clr-primary);
}
</style>
