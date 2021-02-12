## React
* Компоненты, свойства: Props, State
* Метод setState
* Особенности синтаксиса JSX
* Что такое keys в React и чем их важность?
* Презентационные компоненты и компоненты контейнеры
* Что такое refs?
* React router, компонент withRouter
* Компоненты высшего порядка
* Однонаправленный поток данных, работа с событиями в React
* Жизненный цикл компонента, в каких методах жизненного цикла стоит выполнять ajax запросы? В каких стоит "обновлять state на основе props" ?
Оптимизации производительности. 
* Hooks
* articles

#### Компоненты, свойства: Props, State.

* props (намеренно сокращённо от англ. «properties» — свойства), передаются в компонент (служат как параметры функции).

```javascript

//title, created, author, post_hint в данном случае наши пропсы которые в дальнейшем пробрасываються в компонент Title

export function TextContent({title, created, author, post_hint}: ITextContentInterface) {
    return (
        <div className={styles.textContent}>
            <div className={styles.metaData}>
                <div className={styles.userLink}>
                    <a href="#user-url" className={styles.username}>{author}</a>
                </div>
                <span className={styles.createdAt}>
                         <span className={styles.publishedLabel}>опубликовано</span>
                    {moment(created).format("DD-MM-YYYY h:mm:ss")}</span>
            </div>
            <Title title={title} post_hint={post_hint}/>
        </div>
    );
}

export function Title({title, post_hint}: ITitleInterface) {
    return (
        <h2 className={styles.title}>
           <span className={styles.postLink}>{title}</span>
                <Link to="/posts/1" className={styles.postLink}>
                    {title}
                </Link>
            }
        </h2>
    );
}
```

* state — это обычные JavaScript-объекты. Несмотря на то, что оба содержат информацию, которая влияет на то, что увидим после рендера, есть существенное различие: props передаётся в компонент (служат как параметры функции), в то время как state находится внутри компонента (по аналогии с переменными, которые объявлены внутри функции).

#### Метод setState.

Метод setState используется для обновления состояния. Он принимает новое значение состояния и ставит в очередь повторный рендер компонента.

#### Особенности синтаксиса JSX.

JSX — расширение языка JavaScript, представляет собой объекты

Следующие два примера кода эквивалентны между собой:
Babel компилирует JSX в вызовы React.createElement()

```javascript
const element = (
  <h1 className="greeting">
    Привет, мир!
  </h1>
);

const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Привет, мир!'
);
```

React.createElement() проводит некоторые проверки с целью выявить баги в коде, но главное — создаёт объект похожий на такой:

```javascript
// Примечание: этот код несколько упрощён.
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Привет, мир!'
  }
};
```

* Предотвращает атаки, основанные на инъекции кода
  (По умолчанию React DOM экранирует все значения, включённые в JSX перед тем как отрендерить их.)

* Допускает использование любых корректных JavaScript-выражений внутри фигурных скобок
    
  ```javascript
    const element = (
      <h1>
        Здравствуй, {formatName(user)}!
      </h1>
    );
  ```
* Можно использовать внутри выражений if и циклов for, присваивать переменным, передавать функции в качестве аргумента и возвращать из функции.

```javascript
function getGreeting(user) {
     if (user) {
       return <h1>Здравствуй, {formatName(user)}!</h1>;
     }
     return <h1>Здравствуй, незнакомец.</h1>;
   }
```

#### Что такое keys в React и чем их важность?

Ключи помогают React определять, какие элементы были изменены, добавлены или удалены. Их необходимо указывать, чтобы React мог сопоставлять элементы массива с течением времени:

#### Презентационные компоненты и компоненты контейнеры.

Изначально в мире React существовало условное разделение на компоненты содержащие логику, обращение к api, state и 
компоненты занимающиеся отрисовкой (презинтационные компоненты). Это было удобно для консолидации стейта в о дном месте, например для форм, а также для 
шеринга логики между разными презентационными компонентами (или view), например для логики оплаты которую могут использовать разные страницы, попапы или виджеты.
Также для удобства использования контенеров без создания дополнительных компонентов прослоек часто используются *HOC* или *render props*. После появления *hooks*
появился еще один способ шарить логику между компонентами, благодаря написанию кастомных хуков. 
```javascript
const MyComponent = () => {
  const [time] = useClock();
  const [x, y] = useMouseMove();
  // do something with those values
  return (
    <>
      <div>Current time: {time}</div>
      <div>Mouse coordinates: {x},{y}</div>
    </>
  );
}
```

#### Что такое refs?
В обычном потоке данных React родительские компоненты могут взаимодействовать с дочерними только через пропсы. Чтобы модифицировать потомка, вы должны заново отрендерить его с новыми пропсами. Тем не менее, могут возникать ситуации, когда вам требуется императивно изменить дочерний элемент, обойдя обычный поток данных. Подлежащий изменениям дочерний элемент может быть как React-компонентом, так и DOM-элементом. React предоставляет лазейку для обоих случаев.

Рефы дают возможность получить доступ к DOM-узлам или React-элементам, созданным в рендер-методе.
создаются с помощью React.createRef() и прикрепляются к React-элементам через ref атрибут.

Ситуации, в которых использование рефов является оправданным:

* Управление фокусом, выделение текста или воспроизведение медиа.
* Императивный вызов анимаций.
* Интеграция со сторонними DOM-библиотеками.

```JavaScript
// textInput должна быть объявлена здесь, чтобы реф мог иметь к ней доступ
     const textInput = useRef(null);
   
     function handleClick() {
       textInput.current.focus();
     }
   
     return (
       <div>
         <input
           type="text"
           ref={textInput} />
         <input
           type="button"
           value="Фокус на поле для ввода текста"
           onClick={handleClick}
         />
       </div>
     );
   }
```

#### React router, компонент withRouter

Библиотека которая добавляет клиенсткусю маршрутицию.


#### Компоненты высшего порядка.
Компонент высшего порядка — это функция, которая принимает компонент и возвращает новый компонент.

```JavaScript
function logProps(WrappedComponent) {
  return class extends React.Component {
    componentDidUpdate(prevProps) {
      console.log('Текущие пропсы: ', this.props);
      console.log('Предыдущие пропсы: ', prevProps);
    }
    render() {
      // Оборачиваем компонент в контейнер без мутаций. Супер!
      return <WrappedComponent {...this.props} />;
    }
  }
}
```

#### Однонаправленный поток данных, работа с событиями в React.

TODO

### Жизненный цикл компонента, в каких методах жизненного цикла стоит выполнять ajax запросы? В каких стоит "обновлять state на основе props»?

В данный момент существует последовательность жизненных циклов React указанная на рисунке. 
![merge](/img/react_lifecycle1.PNG)  
[ссылка на диаграмму](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)  

**ajax** запросы можно выполнять в методе *componentDidMount()* или в хуке *useEffect*.  

#### Оптимизация
  Для разного синтаксиса сущуствуют различные методы отптимизация производительности React приложений, направленные в основном для
уменьшения количества перерендеринга страниц. 
В классовых компонентах стоит упомянуть:
* **React.PureComponent** - реализует *shouldComponentUpdate()* c поверхностным сравнением props, state (shallow prop and state comparison). 
* **shouldComponentUpdate(nextProps, nextState)** - дает возможность сравнить this.props, this.state с nextProps, nextState и в случае если перерендер не нужен 
вернуть false. 
* **componentDidUpdate(prevProps, prevState, snapshot)** - метода дает возможность сранить prevProps, prevState c this.props, this.state и в случае необходимости сделать запрос по api или this.setState(). 

Для фнкциональных компонентов:  
* Второй аргумент в *useEffect*, следящий за переменными от которых зависит рендер (подробнее в разделе hooks).  
* Второй аргумент в *useMemo*, *useCallback* также следящий за изменениями в переменных от которых зависит рендер. 

Для мониторинга частей рендеринга удобно использовать `React DevTools -> Profiler -> Highlight updates when components render`.  

### Hooks


 **useState** - `const [state, setState] = useState(initialState)` возвращает значение с состоянием и функцию для его обновления. Функция setState используется для обновления состояния. Она принимает новое значение состояния и ставит в очередь повторный рендер компонента. `setState(newState)`  

  **useEffect** - `useEffect(didUpdate)` Функция, переданная в useEffect, будет запущена после того, как рендер будет зафиксирован на экране. Применяется для подписок, таймеров, логирования, обращения к DOM или API (для api лучше использовать мотоды в библиотрках mobX, redux thunk и т.д.). 
  Второй аргумент useEffect отвечает за условия обновления,
  ```
  useEffect(() => { 
    return () => {
      subscription.unsubscribe();
    }; 
  }, [props.source] )
  ```
  Сработает только при изменении *source*, если передать пустой массив [ ] сработает один раз.
  Возвращенная функция (clean-up function) вызывается перед извлечением компонента из UI. 

  **useContext** - `const value = useContext(MyContext)` принимает объект контекста (значение, возвращённое из React.createContext) и возвращает текущее значение контекста для этого контекста. Текущее значение контекста определяется пропом value ближайшего <MyContext.Provider>

  **useRef** - `const refContainer = useRef(initialValue)` возвращает изменяемый ref-объект. Дает доступ к DOM. Если вы передадите React объект рефа с помощью подобного выражения `<div ref={myRef}/>`, React установит собственное свойство .current на соответствующий DOM-узел при каждом его изменении. Получить доступ к объекту - `myRef.current`

  **useCallback** - Возвращает мемоизированный колбэк.
  ```javascript
  const memoizedCallback = useCallback(
    () => {
        doSomething(a, b);
    },
    [a, b],
  )
  ```
  изменяется только, если изменяются значения одной из зависимостей.

  **useMemo** - `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])` Аналогично useCallback но возвращает значение вычислений а не саму функцию.

  **useReducer** - `const [state, dispatch] = useReducer(reducer, initialArg, init)` Принимает редюсер типа (state, action) => newState и возвращает текущее состояние в паре с методом dispatch. Работает по принципу редакса.
  
  
  ### Articles 
  
[How to Reuse Logic with React Hooks](https://rafaelquintanilha.com/how-to-reuse-logic-with-react-hooks/)
