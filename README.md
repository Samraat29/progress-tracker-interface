<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Progress Tracker App</title>
    <style>
        body {
            font-family: 'Times New Roman', serif;
            margin: 12;
            padding: 12;
            background-color: lightblue;
        }

        header {
            background-image: url('https://www.seek.com.au/employer/hiring-advice/assets/customfields/4050/63/2020.008_HIR_What-progress-means-to-employees_940x352.jpg');
            background-size: cover; /* this decides the image size used in header background */
            background-repeat: no-repeat; /* this will help to stop repeating images */
            background-color: #26137b90; /* Background color */
            color: #06becb;
            text-align: center;
            padding: 90px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 40px;
            background-color: #cb7122;
            box-shadow: 0px 0px 15px rgba(223, 228, 60, 0.888);
        }

        footer {
            text-align: center;
            margin-top: 30px;
        }

        .dashboard {
            
        }

        .goal-creation {
            
        }

        .blink {
            animation: blinker 2s linear infinite;
        }

        @keyframes blinker {
            55% {
                opacity: 3;
            }
        }

        .completed {
            text-decoration: line-through;
        }
    </style>
</head>
<body>
    <div id="app">
        <header>
            <h1>Progress Tracker Website</h1>
        </header>
        <div class="container">
            <section class="dashboard">
                <h2>Progress Dashboard Interface</h2>
                <div class="progress-chart">
                </div>
                <div class="task-list">
                    <ul>
                        <li v-for="(goal, goalIndex) in goals" :key="goalIndex">
                            <h3>{{ goal.title }}</h3>
                            <ul>
                                <li v-for="(task, taskIndex) in goal.tasks" :key="taskIndex"
                                    :class="{ completed: task.completed, blink: isDeadlineNear(task.dueDate) }">
                                    <label>
                                        <input type="checkbox" v-model="task.completed"> {{ task.name }}
                                    </label>
                                    <span v-if="task.dueDate">Due Date: {{ task.dueDate }}</span>
                                    <button @click="removeTask(goalIndex, taskIndex)">Remove</button>
                                </li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </section>
            <section class="goal-creation">
                <h2>Create New Goals</h2>
                <form @submit.prevent="addGoal">
                    <input type="text" v-model="newGoalTitle" placeholder="Enter goal title">
                    <input type="text" v-model="newTaskName" placeholder="Enter task name">
                    <input type="date" v-model="newTaskDueDate" placeholder="Enter due date">
                    <button type="submit">Add Goal</button>
                </form>
                <p v-if="duplicateGoal">Goal with this title already exists.</p>
                <p v-if="duplicateTask">Task with this name already exists.</p>
            </section>
        </div>
        <footer>
            <p>&copy; 2023 Progress Tracker App - Made by Samraat</p>
        </footer>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@3.2.20/dist/vue.global.min.js"></script>
    <script>
        const app = Vue.createApp({
            data() {
                return {
                    goals: [
                        {
                            title: 'Learn JavaScript',
                            tasks: [
                                { name: 'Complete Intro to JS course ', completed: false, dueDate: '2023-09-30' },
                                { name: 'Practice JavaScript exercises ', completed: false, dueDate: '2023-10-15' }
                            ]
                        },
                        {
                            title: 'Exercise Regularly',
                            tasks: [
                                { name: 'Run 4Km  ', completed: false, dueDate: '2023-09-25' },
                                { name: 'Do 50 push-ups in a set of 3 ', completed: false, dueDate: '2023-09-30' }
                            ]
                        }
                    ],
                    newGoalTitle: '',
                    newTaskName: '',
                    newTaskDueDate: '',
                    duplicateGoal: false,
                    duplicateTask: false,
                };
            },
            methods: {
                addGoal() {
                    if (this.newGoalTitle.trim() === '') {
                        return;
                    }
                    if (this.goals.some(goal => goal.title === this.newGoalTitle)) {
                        this.duplicateGoal = true;
                        return;
                    }
                    this.duplicateGoal = false;
                    if (this.newTaskName.trim() === '') {
                        return;
                    }
                    if (this.goals.some(goal => goal.tasks.some(task => task.name === this.newTaskName))) {
                        this.duplicateTask = true;
                        return;
                    }
                    this.duplicateTask = false;
                    this.goals.push({
                        title: this.newGoalTitle,
                        tasks: [
                            {
                                name: this.newTaskName,
                                completed: false,
                                dueDate: this.newTaskDueDate
                            }
                        ]
                    });
                    this.newGoalTitle = '';
                    this.newTaskName = '';
                    this.newTaskDueDate = '';
                },
                removeTask(goalIndex, taskIndex) {
                    this.goals[goalIndex].tasks.splice(taskIndex, 1);
                },
                isDeadlineNear(dueDate) {
                    if (dueDate) {
                        const today = new Date();
                        const taskDueDate = new Date(dueDate);
                        const oneDay = 24 * 60 * 60 * 1000;
                        return taskDueDate - today <= oneDay;
                    }
                    return false;
                },
            }
        });

        app.mount('#app');
    </script>
</body>
</html>
