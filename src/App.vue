<template>
  <div class="light-theme-wrapper">
    <!-- Floating Background Shapes -->
    <div class="shape s1"></div>
    <div class="shape s2"></div>

    <header class="hero-section">
      <div class="badge">VUE.JS 3.0</div>
      <h1>Interactive <span class="gradient-text">Playground</span></h1>
      <p>Clean, Colorful & Fully Reactive</p>
    </header>

    <main class="dashboard-grid">
      
      <!-- 1. GREETING CARD (Soft Blue) -->
      <section class="card blue-glow">
        <div class="card-icon">👋</div>
        <h3>Welcome</h3>
        <div class="input-wrapper">
          <input v-model="name" type="text" placeholder="Your name here..." />
        </div>
        <p class="display-text">
          Hi, <span class="highlight-blue">{{ name || 'Friend' }}</span>! 
        </p>
      </section>

      <!-- 2. THE POCKET COUNTER (Warm Orange) -->
      <section class="card orange-glow">
        <h3>Click Counter</h3>
        <div class="counter-engine">
          <button @click="count--" class="btn-round">-</button>
          <div class="number-box">
            <transition name="pop" mode="out-in">
              <span :key="count">{{ count }}</span>
            </transition>
          </div>
          <button @click="count++" class="btn-round">+</button>
        </div>
        <button @click="count = 0" class="btn-link">Reset value</button>
      </section>

      <!-- 3. SMART TODO LIST (Mint Green) -->
      <section class="card green-glow wide">
        <div class="card-header">
          <h3>Task Manager</h3>
          <div class="pill-stats">{{ completedCount }} / {{ todos.length }} Done</div>
        </div>

        <div class="todo-input-group">
          <input v-model="newTodo" @keyup.enter="addTodo" placeholder="What's your next goal?" />
          <button @click="addTodo" class="btn-add">Add</button>
        </div>

        <div class="list-container">
          <transition-group name="stagger">
            <div v-for="(todo, index) in todos" :key="todo.id" class="list-item">
              <label class="checkbox-container">
                <input type="checkbox" v-model="todo.completed" />
                <span class="checkmark"></span>
              </label>
              <span :class="['item-text', { struck: todo.completed }]">{{ todo.text }}</span>
              <button @click="removeTodo(index)" class="btn-remove">✕</button>
            </div>
          </transition-group>
        </div>
      </section>

      <!-- 4. COLOR LAB (Purple/Indigo) -->
      <section class="card purple-glow">
        <h3>Style Lab</h3>
        <div class="lab-grid">
          <div class="control">
            <span>Color</span>
            <input type="color" v-model="boxColor" />
          </div>
          <div class="control">
            <span>Size</span>
            <input type="range" v-model="boxSize" min="60" max="150" />
          </div>
        </div>
        <div class="box-preview">
          <div 
            class="vibrant-box"
            :style="{ backgroundColor: boxColor, width: boxSize + 'px', height: boxSize + 'px', borderRadius: (boxSize/5) + 'px' }"
          ></div>
        </div>
      </section>

      <!-- 5. SECRET TOGGLE (Pink) -->
      <section class="card pink-glow">
        <h3>Mystery Box</h3>
        <button @click="showSecret = !showSecret" class="btn-gradient">
          {{ showSecret ? 'Hide Message' : 'Reveal Message' }}
        </button>
        <transition name="fade-slide">
          <div v-if="showSecret" class="secret-info">
            🎉 Vue makes UI fun!
          </div>
        </transition>
      </section>

    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      count: 0,
      showSecret: false,
      newTodo: '',
      todos: [
        { id: 1, text: 'Learn Vue Basics', completed: true },
        { id: 2, text: 'Create something beautiful', completed: false }
      ],
      boxColor: '#6366f1',
      boxSize: 100
    }
  },
  computed: {
    completedCount() {
      return this.todos.filter(t => t.completed).length
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({ id: Date.now(), text: this.newTodo, completed: false });
        this.newTodo = '';
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');

:root {
  --primary: #42b883;
  --blue: #3b82f6;
  --orange: #f59e0b;
  --pink: #ec4899;
  --purple: #8b5cf6;
  --bg: #f8fafc;
  --text-main: #1e293b;
}

body {
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  background: var(--bg);
  color: var(--text-main);
}

.light-theme-wrapper {
  min-height: 100vh;
  padding: 50px 20px;
  position: relative;
  overflow: hidden;
}

/* Background Shapes */
.shape {
  position: absolute;
  filter: blur(80px);
  z-index: -1;
  border-radius: 50%;
}
.s1 { width: 300px; height: 300px; background: #dcfce7; top: -50px; left: -50px; }
.s2 { width: 400px; height: 400px; background: #dbeafe; bottom: -100px; right: -50px; }

/* Header */
.hero-section { text-align: center; margin-bottom: 50px; }
.badge {
  display: inline-block;
  padding: 5px 15px;
  background: white;
  border-radius: 20px;
  font-size: 0.7rem;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0,0,0,0.05);
  color: var(--primary);
  margin-bottom: 10px;
}
h1 { font-size: 3rem; margin: 0; font-weight: 800; }
.gradient-text {
  background: linear-gradient(to right, var(--primary), var(--blue));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Grid Layout */
.dashboard-grid {
  max-width: 1000px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 25px;
}
.wide { grid-column: span 2; }
@media (max-width: 800px) { .wide { grid-column: span 1; } }

/* Card Styling */
.card {
  background: white;
  border-radius: 30px;
  padding: 30px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.03);
  transition: transform 0.3s ease;
  position: relative;
  overflow: hidden;
}
.card:hover { transform: translateY(-5px); }

.blue-glow { border-top: 6px solid var(--blue); }
.orange-glow { border-top: 6px solid var(--orange); }
.green-glow { border-top: 6px solid var(--primary); }
.purple-glow { border-top: 6px solid var(--purple); }
.pink-glow { border-top: 6px solid var(--pink); }

h3 { margin-top: 0; color: #64748b; font-size: 1rem; text-transform: uppercase; letter-spacing: 1px; }

/* Components */
input[type="text"] {
  width: 100%;
  padding: 12px 20px;
  border: 2px solid #f1f5f9;
  border-radius: 15px;
  font-family: inherit;
  font-size: 1rem;
  outline: none;
  transition: border 0.3s;
}
input:focus { border-color: var(--blue); }

.display-text { font-size: 1.2rem; margin-top: 20px; }
.highlight-blue { color: var(--blue); font-weight: 700; }

.counter-engine {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  margin: 20px 0;
}
.btn-round {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: none;
  background: #f1f5f9;
  font-size: 1.5rem;
  cursor: pointer;
  transition: 0.2s;
}
.btn-round:hover { background: var(--orange); color: white; }
.number-box { font-size: 3rem; font-weight: 700; min-width: 60px; text-align: center; }

/* Todo List */
.card-header { display: flex; justify-content: space-between; align-items: center; }
.pill-stats { background: #f0fdf4; color: var(--primary); padding: 5px 15px; border-radius: 20px; font-weight: 600; }
.todo-input-group { display: flex; gap: 10px; margin: 20px 0; }
.btn-add { background: var(--primary); color: white; border: none; padding: 10px 25px; border-radius: 12px; cursor: pointer; font-weight: 600; }

.list-item {
  display: flex;
  align-items: center;
  padding: 15px;
  background: #f8fafc;
  border-radius: 15px;
  margin-bottom: 10px;
  gap: 15px;
}
.struck { text-decoration: line-through; opacity: 0.5; }
.btn-remove { margin-left: auto; border: none; background: transparent; color: #cbd5e1; cursor: pointer; font-size: 1.2rem; }
.btn-remove:hover { color: #f87171; }

/* Visual Lab */
.lab-grid { display: flex; gap: 20px; margin-bottom: 20px; }
.control { flex: 1; display: flex; flex-direction: column; gap: 5px; }
.box-preview { display: flex; justify-content: center; align-items: center; height: 160px; }
.vibrant-box { transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); box-shadow: 0 10px 20px rgba(0,0,0,0.1); }

/* Animations */
.pop-enter-active, .pop-leave-active { transition: all 0.2s ease; }
.pop-enter-from { transform: scale(1.5); opacity: 0; }
.pop-leave-to { transform: scale(0.5); opacity: 0; }

.fade-slide-enter-active { transition: all 0.3s ease; }
.fade-slide-enter-from { opacity: 0; transform: translateY(-10px); }

.stagger-enter-active, .stagger-leave-active { transition: all 0.3s ease; }
.stagger-enter-from { opacity: 0; transform: translateX(-20px); }
.stagger-leave-to { opacity: 0; transform: scale(0.95); }

/* Checkbox */
.checkbox-container { display: block; position: relative; width: 20px; height: 20px; cursor: pointer; }
.checkbox-container input { opacity: 0; width: 0; height: 0; }
.checkmark { position: absolute; top: 0; left: 0; height: 20px; width: 20px; background-color: #e2e8f0; border-radius: 6px; }
.checkbox-container:hover input ~ .checkmark { background-color: #cbd5e1; }
.checkbox-container input:checked ~ .checkmark { background-color: var(--primary); }
.checkmark:after { content: ""; position: absolute; display: none; left: 7px; top: 3px; width: 5px; height: 10px; border: solid white; border-width: 0 2px 2px 0; transform: rotate(45deg); }
.checkbox-container input:checked ~ .checkmark:after { display: block; }

.btn-gradient {
  width: 100%;
  padding: 12px;
  border: none;
  border-radius: 15px;
  background: linear-gradient(to right, #ec4899, #f43f5e);
  color: white;
  font-weight: 700;
  cursor: pointer;
}
.secret-info {
  margin-top: 15px;
  padding: 15px;
  background: #fff1f2;
  color: #be123c;
  border-radius: 15px;
  text-align: center;
  font-weight: 600;
}
</style>