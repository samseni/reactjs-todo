import { useState } from 'react';
import './App.css';


export default function TodoApp() {
  const [tasks, setTasks] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [filter, setFilter] = useState('all');

  const addTask = () => {
    if (inputValue.trim() !== '') {
      const newTask = {
        id: Date.now(),
        text: inputValue.trim(),
        completed: false
      };
      setTasks([...tasks, newTask]);
      setInputValue('');
    }
  };

  const toggleTask = (id) => {
    setTasks(tasks.map(task =>
      task.id === id ? { ...task, completed: !task.completed } : task
    ));
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter(task => task.id !== id));
  };

  const totalTasks = tasks.length;
  const completedTasks = tasks.filter(task => task.completed).length;
  const pendingTasks = totalTasks - completedTasks;

  const filteredTasks = tasks.filter(task => {
    if (filter === 'completed') return task.completed;
    if (filter === 'pending') return !task.completed;
    return true;
  });

  return (
    <>
      <style jsx>{`
        .todo-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.main-wrapper {
  width: 100%;
  max-width: 512px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header {
  text-align: center;
  margin-bottom: 48px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title {
  font-size: 3rem;
  font-weight: bold;
  color: white;
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  text-shadow: 0 4px 20px rgba(0,0,0,0.3);
}

.subtitle {
  color: rgba(255,255,255,0.9);
  font-size: 1.25rem;
  text-align: center;
}

.main-card {
  background: rgba(255,255,255,0.95);
  backdrop-filter: blur(20px);
  border-radius: 24px;
  box-shadow: 0 25px 50px rgba(0,0,0,0.25);
  padding: 32px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid rgba(255,255,255,0.2);
}

.input-section {
  display: flex;
  gap: 16px;
  margin-bottom: 40px;
  width: 100%;
  justify-content: center;
  align-items: center;
}

.main-input {
  flex: 1;
  padding: 16px 24px;
  border: 2px solid #e2e8f0;
  border-radius: 16px;
  outline: none;
  color: #4a5568;
  font-size: 1.125rem;
  text-align: center;
  transition: all 0.3s ease;
  background: white;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.main-input:focus {
  border-color: #4299e1;
  box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.1), 0 4px 20px rgba(0,0,0,0.15);
  transform: translateY(-2px);
}

.add-button {
  padding: 16px 32px;
  background: linear-gradient(135deg, #4299e1, #3182ce);
  color: white;
  border: none;
  border-radius: 16px;
  font-weight: 600;
  font-size: 1.125rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 8px 25px rgba(66, 153, 225, 0.4);
}

.add-button:hover {
  background: linear-gradient(135deg, #3182ce, #2c5282);
  transform: translateY(-3px);
  box-shadow: 0 12px 35px rgba(66, 153, 225, 0.6);
}

.add-button:active {
  transform: translateY(-1px);
}

.stats-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 24px;
  margin-bottom: 40px;
  width: 100%;
}

.stat-circle {
  width: 96px;
  height: 96px;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.stat-circle::before {
  content: '';
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: linear-gradient(45deg, rgba(255,255,255,0.5), transparent);
  border-radius: 50%;
  z-index: 0;
}

.stat-circle > * {
  position: relative;
  z-index: 1;
}

.stat-total {
  background: linear-gradient(135deg, #bee3f8, #90cdf4);
  box-shadow: 0 8px 25px rgba(66, 153, 225, 0.3);
}

.stat-done {
  background: linear-gradient(135deg, #c6f6d5, #9ae6b4);
  box-shadow: 0 8px 25px rgba(72, 187, 120, 0.3);
}

.stat-pending {
  background: linear-gradient(135deg, #fefcbf, #faf089);
  box-shadow: 0 8px 25px rgba(236, 201, 75, 0.3);
}

.stat-circle:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 15px 40px rgba(0,0,0,0.2);
}

.stat-emoji {
  font-size: 1.5rem;
  margin-bottom: 4px;
}

.stat-text {
  font-weight: bold;
  font-size: 0.875rem;
  text-align: center;
}

.filters-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 16px;
  margin-bottom: 40px;
  width: 100%;
}

.filter-button {
  width: 96px;
  height: 80px;
  border-radius: 16px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.filter-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
  transition: left 0.5s;
}

.filter-button:hover::before {
  left: 100%;
}

.filter-active {
  color: white;
  box-shadow: 0 8px 25px rgba(0,0,0,0.3);
  transform: translateY(-2px);
}

.filter-inactive {
  background: #f7fafc;
  color: #4a5568;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.filter-inactive:hover {
  background: #edf2f7;
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.15);
}

.filter-all-active {
  background: linear-gradient(135deg, #4299e1, #3182ce);
}

.filter-pending-active {
  background: linear-gradient(135deg, #ed8936, #dd6b20);
}

.filter-completed-active {
  background: linear-gradient(135deg, #48bb78, #38a169);
}

.tasks-container {
  width: 100%;
  max-height: 384px;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: #cbd5e0 transparent;
}

.tasks-container::-webkit-scrollbar {
  width: 6px;
}

.tasks-container::-webkit-scrollbar-track {
  background: transparent;
}

.tasks-container::-webkit-scrollbar-thumb {
  background: #cbd5e0;
  border-radius: 3px;
}

.tasks-container::-webkit-scrollbar-thumb:hover {
  background: #a0aec0;
}

.empty-state {
  text-align: center;
  padding: 64px 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.empty-emoji {
  font-size: 5rem;
  margin-bottom: 24px;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}

.empty-text {
  color: #718096;
  font-size: 1.25rem;
  text-align: center;
  max-width: 300px;
  margin: 0 auto;
  line-height: 1.6;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 16px;
  border-radius: 16px;
  border: 2px solid;
  transition: all 0.3s ease;
  width: 100%;
  margin-bottom: 16px;
  position: relative;
  overflow: hidden;
  animation: slideIn 0.3s ease-out;
}

.task-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
  transition: left 0.6s;
}

.task-item:hover::before {
  left: 100%;
}

.task-completed {
  background: linear-gradient(135deg, #f0fff4, #c6f6d5);
  border-color: #9ae6b4;
  opacity: 0.8;
}

.task-pending {
  background: linear-gradient(135deg, #ffffff, #f7fafc);
  border-color: #e2e8f0;
}

.task-pending:hover {
  border-color: #4299e1;
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.1);
}

.task-checkbox {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  border: 2px solid;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.checkbox-completed {
  background: linear-gradient(135deg, #48bb78, #38a169);
  border-color: #38a169;
  color: white;
  box-shadow: 0 4px 15px rgba(72, 187, 120, 0.4);
}

.checkbox-pending {
  border-color: #cbd5e0;
  background: white;
}

.checkbox-pending:hover {
  border-color: #48bb78;
  background: #f0fff4;
  transform: scale(1.1);
}

.task-text-container {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 16px;
}

.task-text {
  font-size: 1.125rem;
  text-align: center;
  transition: all 0.3s ease;
}

.text-completed {
  text-decoration: line-through;
  color: #718096;
}

.text-pending {
  color: #2d3748;
}

.delete-button {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #fed7d7, #feb2b2);
  color: #e53e3e;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 1.25rem;
}

.delete-button:hover {
  background: linear-gradient(135deg, #e53e3e, #c53030);
  color: white;
  transform: scale(1.1) rotate(90deg);
  box-shadow: 0 4px 15px rgba(229, 62, 62, 0.4);
}

.progress-section {
  margin-top: 32px;
  padding-top: 24px;
  border-top: 1px solid #e2e8f0;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.progress-header {
  text-align: center;
  margin-bottom: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.progress-percentage {
  font-size: 1.5rem;
  font-weight: bold;
  color: #2d3748;
  margin-bottom: 8px;
  text-align: center;
}

.progress-bar-container {
  width: 100%;
  max-width: 320px;
  background: #e2e8f0;
  border-radius: 12px;
  height: 12px;
  overflow: hidden;
  margin: 0 auto;
  box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
}

.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, #48bb78, #38a169);
  transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  border-radius: 12px;
  position: relative;
  overflow: hidden;
}

.progress-bar::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.6), transparent);
  animation: shimmer 2s infinite;
}

@keyframes shimmer {
  0% { left: -100%; }
  100% { left: 100%; }
}

@keyframes slideIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

.add-button:focus {
  animation: pulse 0.3s ease-in-out;
}

      `}</style>

      <div className="todo-container">
        <div className="main-wrapper">
          <div className="header">
            <h1 className="title">
              <span style={{fontSize: '4rem'}}>⭐</span>
              Todo List
            </h1>
            <p className="subtitle">Stay organised and productive</p>
          </div>

          <div className="main-card">
            <div className="input-section">
              <input
                type="text"
                value={inputValue}
                onChange={(e) => setInputValue(e.target.value)}
                onKeyPress={(e) => e.key === 'Enter' && addTask()}
                placeholder="What needs to be done?"
                className="main-input"
              />
              <button
                onClick={addTask}
                className="add-button"
              >
                + Add
              </button>
            </div>

            <div className="stats-container">
              <div className="stat-circle stat-total">
                <div className="stat-emoji">📊</div>
                <div className="stat-text" style={{color: '#2b6cb0'}}>Total: {totalTasks}</div>
              </div>
              <div className="stat-circle stat-done">
                <div className="stat-emoji">✅</div>
                <div className="stat-text" style={{color: '#276749'}}>Done: {completedTasks}</div>
              </div>
              <div className="stat-circle stat-pending">
                <div className="stat-emoji">⏳</div>
                <div className="stat-text" style={{color: '#975a16'}}>Pending: {pendingTasks}</div>
              </div>
            </div>

            <div className="filters-container">
              <button
                onClick={() => setFilter('all')}
                className={`filter-button ${
                  filter === 'all' 
                    ? 'filter-active filter-all-active' 
                    : 'filter-inactive'
                }`}
              >
                <div style={{fontSize: '1.5rem', marginBottom: '4px'}}>🔍</div>
                <div style={{fontSize: '0.875rem'}}>All</div>
              </button>
              <button
                onClick={() => setFilter('pending')}
                className={`filter-button ${
                  filter === 'pending' 
                    ? 'filter-active filter-pending-active' 
                    : 'filter-inactive'
                }`}
              >
                <div style={{fontSize: '1.5rem', marginBottom: '4px'}}>⏳</div>
                <div style={{fontSize: '0.875rem'}}>Pending</div>
              </button>
              <button
                onClick={() => setFilter('completed')}
                className={`filter-button ${
                  filter === 'completed' 
                    ? 'filter-active filter-completed-active' 
                    : 'filter-inactive'
                }`}
              >
                <div style={{fontSize: '1.5rem', marginBottom: '4px'}}>✅</div>
                <div style={{fontSize: '0.875rem'}}>Done</div>
              </button>
            </div>

            <div className="tasks-container">
              {filteredTasks.length === 0 ? (
                <div className="empty-state">
                  <div className="empty-emoji">📝</div>
                  <p className="empty-text">
                    {filter === 'all' 
                      ? 'No tasks yet. Add your first task above!' 
                      : filter === 'completed'
                      ? 'No completed tasks yet. Get started!'
                      : 'No pending tasks. Well done! 🎉'
                    }
                  </p>
                </div>
              ) : (
                filteredTasks.map(task => (
                  <div 
                    key={task.id} 
                    className={`task-item ${
                      task.completed ? 'task-completed' : 'task-pending'
                    }`}
                  >
                    <div>
                      <button
                        onClick={() => toggleTask(task.id)}
                        className={`task-checkbox ${
                          task.completed ? 'checkbox-completed' : 'checkbox-pending'
                        }`}
                      >
                        {task.completed && <span style={{fontSize: '1.125rem', fontWeight: 'bold'}}>✓</span>}
                      </button>
                    </div>
                    <div className="task-text-container">
                      <span className={`task-text ${
                        task.completed ? 'text-completed' : 'text-pending'
                      }`}>
                        {task.text}
                      </span>
                    </div>
                    <div>
                      <button
                        onClick={() => deleteTask(task.id)}
                        className="delete-button"
                      >
                        ×
                      </button>
                    </div>
                  </div>
                ))
              )}
            </div>

            {totalTasks > 0 && (
              <div className="progress-section">
                <div className="progress-header">
                  <div className="progress-percentage">
                    {Math.round((completedTasks / totalTasks) * 100)}% Complete
                  </div>
                </div>
                <div className="progress-bar-container">
                  <div 
                    className="progress-bar"
                    style={{ width: `${(completedTasks / totalTasks) * 100}%` }}
                  ></div>
                </div>
              </div>
            )}
          </div>
        </div>
      </div>
    </>
  );
}
