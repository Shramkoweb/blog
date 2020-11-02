---
title: Реакт контекст и хуки  
date: "2020-11-02"
description: "Что такое контекст и как его использовать с хуками"
---

#### Какую проблему решаем?

> В реакте используется философия одностороннего потока данных.

Например кнопке которая находится где-то в глубине приложения нужно получить `props`
от самого верхнего `App`, для этого нам прийдётся этот `props` кидать через всех детей на этом пути.

```js
const Button = (props) => {
    const { locale } = props;

    // Если локаль 'en' текст кнопки будет на английском
    if (locale === 'en') {
        return <button type="button">Click me pls!</button>;
    }
   
    // Иначе на русском
    return <button type="button">Нажми меня!</button>;
};

const Header = (props) => {
    // Header должен пробросить props.locale 
    // хотя ему он не нужен
    return <Button locale={props.locale}/>;
};

function App() {
    const [locale, setLocal] = React.useState('en');

    return (
        <div className="App">
            <Header locale={locale}/>
        </div>
    );
}
```

Эта проблема становится очень заметной на больших проектах, когда компонентов много и они глубоко в недрах приложения.

#### Как нам поможет Контекст?

> Контекст позволяет передавать данные через дерево 
> компонентов без необходимости передавать пропсы на промежуточных уровнях.

Давайте перепишем наше приложение используя контекст.

```js
// Создаем контекст с данными
const LocaleContext = React.createContext();
```
    
Внутри `App` Оборачиваем все приложение в `LocaleContext.Provider` 
значение к которому хотим иметь дальше доступ передаем в `value` у нас это `locale`.
    
```js
function App() {
    const [locale, setLocal] = React.useState('en');

    // Оборачиваем все приложение в LocaleContext.Provider
    return (
        <LocaleContext.Provider value={locale}>
            <Header/>
        </LocaleContext.Provider>
    );
}
```
    
Наша кнопка уже не получает никаких пропсов, нужные данные она берет сама из контекста.

```js
const Button = () => {
    // Вызываем хук useContext и в него передаем
    // LocaleContext который мы создали ранее
    const locale = React.useContext(LocaleContext);

    if (locale === 'en') {
        return <button type="button">Click me pls!</button>;
    }

    return <button type="button">Нажми меня!</button>;
};
```
    
`Header` стал независимый и ничего не знает о пропсах для `Button`.

```js
const Header = () => {
    // Header ничего не знает о пропсах нужных кнопке
    return <Button/>;
};
```

Финальное приложение:

```js
const LocaleContext = React.createContext();

const Button = () => {
    const locale = React.useContext(LocaleContext);

    if (locale === 'en') {
        return <button type="button">Click me pls!</button>;
    }

    return <button type="button">Нажми меня!</button>;
};

const Header = () => {
    return <Button/>;
};

function App() {
    const [locale, setLocal] = React.useState('en');

    return (
        <LocaleContext.Provider value={locale}>
            <Header/>
        </LocaleContext.Provider>
    );
}
```    
