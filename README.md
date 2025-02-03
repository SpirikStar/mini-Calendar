# Модуль: Интерактивный календарь

## Описание модуля
- Этот модуль представляет собой интерактивный календарь, который позволяет:
- Просматривать текущий месяц и переключаться между месяцами.
- Выбирать дату из календаря.
- Отображать диапазон недели (начало и конец недели) для выбранной даты.
- Переключаться между неделями (вперед и назад).
- Сбрасывать выбор на текущую дату.

# Основные функции модуля:
## 1. Инициализация календаря
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const currentMonthElement = document.getElementById('currentMonth');
    const calendarDaysElement = document.getElementById('calendarDays');
    const prevMonthButton = document.getElementById('prevMonth');
    const nextMonthButton = document.getElementById('nextMonth');
    const resetButton = document.getElementById('resetButton');
    const prevWeekButton = document.getElementById('prevWeek');
    const nextWeekButton = document.getElementById('nextWeek');
    const weekRangeElement = document.getElementById('weekRange');
    let currentDate = new Date();
    const today = new Date();
    let selectedDate = new Date(); // Текущая дата по умолчанию

    renderCalendar(); // Отрисовка календаря.
    updateWeekDisplay(); // Отображение текущей недели.

    // Обработчики событий для кнопок управления.
    prevMonthButton.addEventListener('click', () => {
        currentDate.setMonth(currentDate.getMonth() - 1);
        renderCalendar();
    });

    nextMonthButton.addEventListener('click', () => {
        currentDate.setMonth(currentDate.getMonth() + 1);
        renderCalendar();
    });

    resetButton.addEventListener('click', () => {
        currentDate = new Date(today);
        selectedDate = new Date(today);
        renderCalendar();
        updateWeekDisplay();
    });

    prevWeekButton.addEventListener('click', () => {
        if (selectedDate) {
            selectedDate.setDate(selectedDate.getDate() - 7);
            updateWeekDisplay();
        }
    });

    nextWeekButton.addEventListener('click', () => {
        if (selectedDate) {
            selectedDate.setDate(selectedDate.getDate() + 7);
            updateWeekDisplay();
        }
    });
});
```
## 2. Отрисовка календаря
```javascript
function renderCalendar() {
    const currentMonthElement = document.getElementById('currentMonth');
    const calendarDaysElement = document.getElementById('calendarDays');
    const options = { year: 'numeric', month: 'long' };
    currentMonthElement.textContent = currentDate.toLocaleDateString('ru-RU', options);

    calendarDaysElement.innerHTML = '';

    const firstDayOfMonth = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1).getDay();
    const daysInMonth = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0).getDate();
    const adjustedFirstDay = (firstDayOfMonth === 0 ? 7 : firstDayOfMonth);

    for (let i = 0; i < adjustedFirstDay - 1; i++) {
        const emptyCell = document.createElement('div');
        emptyCell.classList.add('disabled');
        calendarDaysElement.appendChild(emptyCell);
    }

    for (let i = 1; i <= daysInMonth; i++) {
        const dayCell = document.createElement('div');
        dayCell.textContent = i;

        if (i === today.getDate() &&
            currentDate.getMonth() === today.getMonth() &&
            currentDate.getFullYear() === today.getFullYear()) {
            dayCell.classList.add('current-day');
        }

        dayCell.addEventListener('click', () => {
            selectedDate = new Date(currentDate.getFullYear(), currentDate.getMonth(), i);
            updateWeekDisplay();
            renderCalendar();
        });

        if (selectedDate &&
            selectedDate.getDate() === i &&
            selectedDate.getMonth() === currentDate.getMonth() &&
            selectedDate.getFullYear() === currentDate.getFullYear()) {
            dayCell.classList.add('selected');
        }

        calendarDaysElement.appendChild(dayCell);
    }
}
```

## 3. Обновление отображения недели
```javascript
function updateWeekDisplay() {
    const weekRangeElement = document.getElementById('weekRange');

    if (selectedDate) {
        const dayOfWeek = selectedDate.getDay() || 7;
        const startOfWeek = new Date(selectedDate);
        startOfWeek.setDate(selectedDate.getDate() - dayOfWeek + 1);
        const endOfWeek = new Date(startOfWeek);
        endOfWeek.setDate(startOfWeek.getDate() + 6);

        const formatDate = (date) => {
            const options = { day: 'numeric', month: 'long' };
            return date.toLocaleDateString('ru-RU', options);
        };

        const formatFullDate = (date) => {
            const options = { year: 'numeric', day: 'numeric', month: 'long' };
            return date.toLocaleDateString('ru-RU', options);
        };

        if (startOfWeek.getMonth() === endOfWeek.getMonth()) {
            weekRangeElement.textContent = `${formatDate(startOfWeek)} - ${formatFullDate(endOfWeek)}`;
        } else {
            weekRangeElement.textContent = `${formatDate(startOfWeek)} - ${formatFullDate(endOfWeek)}`;
        }
    } else {
        weekRangeElement.textContent = 'Выберите дату';
    }
}
```
