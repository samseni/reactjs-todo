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
        /* Same CSS you posted, keep it here in full */
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
