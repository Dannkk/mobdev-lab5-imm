# ToDo-лист на React Native

## Цель:

Разработать мобильное приложение ToDo-лист на React Native.

## Функциональные требования:

- Список задач:
    - Отображение списка задач с возможностью отметки как выполненной (checkbox или аналог).
    - Возможность удаления задачи (удаление через кнопку).
      Поддержка фильтров: все, активные, выполненные\*
- Добавление задачи:
    - Форма с полем ввода текста.
    - Кнопка для сохранения задачи.

## Параллели между React Web и React Native

| React Web                            | React Native               |
| ------------------------------------ | -------------------------- |
| &lt;div&gt;, &lt;span&gt;, &lt;p&gt; | &lt;View&gt;, &lt;Text&gt; |
| CSS                                  | StyleSheet.create()        |
| DOM-события (onClick)                | События (onPress)          |

## Работа с полем ввода

| React Web                        | React Native                |
| -------------------------------- | --------------------------- |
| &lt;input type="text" /&gt;      | &lt;TextInput /&gt;         |
| Атрибут value и событие onChange | Пропсы value и onChangeText |
| Событие onSubmit                 | Событие onSubmitEditing     |

### Пример применение объекта поля ввода

```tsx
<View style={styles.container}>
    <TextInput
        style={styles.input}
        placeholder="Введите задачу"
        value={task}
        onChangeText={setTask} // Обработка изменения текста
        onSubmitEditing={handleSubmit} // Отправка по нажатию Enter (на мобильной клавиатуре)
        returnKeyType="done" // Настройка кнопки "Enter" на клавиатуре
    />
    <Button title="Добавить" onPress={handleSubmit} />
</View>
```

### Рендер элементов

FlatList рендерит только видимые элементы и немного элементов вокруг них (offscreen), что снижает нагрузку на память.

#### Пример использование FlatList

```tsx
import React from 'react'
import { View, Text, FlatList, StyleSheet, Image } from 'react-native'

// Данные для списка контактов
const contacts = [
    {
        id: '1',
        name: 'Алексей Иванов',
        phone: '+7 912 345-67-89',
        avatar: 'https://via.placeholder.com/50',
    },
    {
        id: '2',
        name: 'Мария Смирнова',
        phone: '+7 923 456-78-90',
        avatar: 'https://via.placeholder.com/50',
    },
    {
        id: '3',
        name: 'Дмитрий Кузнецов',
        phone: '+7 934 567-89-01',
        avatar: 'https://via.placeholder.com/50',
    },
    {
        id: '4',
        name: 'Ольга Попова',
        phone: '+7 945 678-90-12',
        avatar: 'https://via.placeholder.com/50',
    },
]

export default function App() {
    return (
        <View style={styles.container}>
            <Text style={styles.header}>Список контактов</Text>

            <FlatList
                data={contacts} // 📋 Источник данных для списка
                keyExtractor={(item) => item.id} // 🔑 Уникальный ключ для каждого элемента
                renderItem={(
                    { item }, // 🎨 Отображение каждого элемента
                ) => (
                    <View style={styles.contactItem}>
                        <Image
                            source={{ uri: item.avatar }}
                            style={styles.avatar}
                        />
                        <View>
                            <Text style={styles.name}>{item.name}</Text>
                            <Text style={styles.phone}>{item.phone}</Text>
                        </View>
                    </View>
                )}
            />
        </View>
    )
}
```

## Чекбокс

Switch — это компонент-переключатель (toggle), который выполняет ту же роль, что и чекбокс.

| Пропс         | Описание                               | Пример                        |
| ------------- | -------------------------------------- | ----------------------------- |
| value         | Состояние (вкл/выкл)                   | value={isEnabled}             |
| onValueChange | Функция-обработчик изменения состояния | onValueChange={toggleSwitch}  |
| thumbColor    | Цвет ползунка                          | thumbColor="#4CAF50"          |
| trackColor    | Цвет фона для состояний true и false   | trackColor={{ true: 'blue' }} |

### Использование Switch

```tsx
export default function App() {
  const [isEnabled, setIsEnabled] = useState(false);

  const toggleSwitch = () => setIsEnabled((prev) => !prev);

  return (
    <View style={styles.container}>
      <Text style={styles.label}>Согласен с условиями:</Text>
      <Switch

        value={isEnabled}       // Значение (вкл/выкл)
        onValueChange={toggleSwitch} // Обработчик изменения
        thumbColor={isEnabled ? '#4CAF50' : '#f4f3f4'} // Цвет ползунка
        trackColor={{ false: '#767577', true: '#81b0ff' }} // Цвет фона
      />
    </View>
  );
}
```
