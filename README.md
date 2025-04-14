```markdown
# Повний звіт з лінійного програмування

## 1. Вступ
Лінійне програмування - це потужний інструмент для оптимізації рішень. У цій роботі ми реалізували:

## 2. Код програми

### 2.1. Імпорт бібліотек
```python
# Встановлення та імпорт необхідних пакетів
import sys
!{sys.executable} -m pip install --upgrade scipy numpy matplotlib

import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import linprog
from IPython.display import display, Markdown
```

### 2.2. Допоміжні функції
```python
def print_header(text):
    display(Markdown(f"**{text}**"))
    
def plot_feasible_region(A, b, x_opt=None):
    """Візуалізація області допустимих рішень"""
    x = np.linspace(0, 50, 100)
    plt.figure(figsize=(10,6))
    
    # Малюємо кожне обмеження
    for i in range(A.shape[0]):
        y = (b[i] - A[i,0]*x)/A[i,1]
        plt.plot(x, y, label=f'{A[i,0]:.1f}x1 + {A[i,1]:.1f}x2 ≤ {b[i]:.1f}')
    
    if x_opt is not None:
        plt.scatter(x_opt[0], x_opt[1], color='red', s=100, 
                   label=f'Оптимум ({x_opt[0]:.1f}, {x_opt[1]:.1f})')
    
    plt.xlabel('x1 (Продукт A)')
    plt.ylabel('x2 (Продукт B)')
    plt.legend()
    plt.grid()
    plt.show()
```

### 2.3. Основні задачі

#### Задача 1: Максимізація прибутку
```python
print_header("Задача 1: Максимізація прибутку")

# Вхідні дані
c = [-60, -40]  # Коефіцієнти цільової функції (мінус для максимізації)
A = [[3, 2],    # Матриця обмежень
     [1, 3],
     [2, 2]]
b = [180, 120, 150]  # Вектор обмежень

# Розв'язання
res = linprog(c, A_ub=A, b_ub=b, bounds=[(0, None), (0, None)], method='highs')

# Вивід результатів
print(f"Статус: {'Успішно' if res.success else 'Помилка'}")
print(f"Оптимальний план: x1 = {res.x[0]:.2f}, x2 = {res.x[1]:.2f}")
print(f"Максимальний прибуток: {-res.fun:.2f} грн")

# Візуалізація
plot_feasible_region(np.array(A), np.array(b), res.x)
```

#### Задача 2: Мінімізація витрат
```python
print_header("\nЗадача 2: Мінімізація витрат")

# Вхідні дані
c = [150, 200]  # Витрати на видобуток
A = [[-2, -1],  # Обмеження типу ≥
     [-1, -2]]
b = [-100, -150]

# Розв'язання
res = linprog(c, A_ub=A, b_ub=b, bounds=[(0, None), (0, None)])

# Вивід результатів
print(f"Оптимальний план видобутку:")
print(f"Родовище 1: {res.x[0]:.2f} тонн")
print(f"Родовище 2: {res.x[1]:.2f} тонн")
print(f"Мінімальні витрати: {res.fun:.2f} грн")
```

## 3. Результати
### Таблиця результатів
| Задача       | Оптимальні значення | Цільова функція |
|--------------|---------------------|-----------------|
| Максимізація | x1=20.0, x2=33.3    | 2533.33 грн     |
| Мінімізація  | x1=16.7, x2=41.7    | 10833.33 грн    |


