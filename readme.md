# React 18. Что нового?

## Новые фичи:

### React
- **useId** - это новый хук для создания уникальных идентификаторов как на клиенте, так и на сервере, избегая при этом hydration несоответствий
- **startTransition** - предназначается для обновления состояния, которое влечет за собой тяжелые вычисления. Например, фильтрация списка. Это позволяет значительно улучшить пользовательский ввод и отклик интерфейса, т.к. помечает тяжелые обновления компонента как «переходы» — transitions.
  startTransition полезен, если вы хотите сделать пользовательский ввод быстрым, не было фриза UI, а несрочные операции выполнялись на фоне.
```
import { startTransition } from 'react';

// Срочное (urgent) обновление: отображаем введенный текст
setInputValue(input);

// Помечаем обновления состояний как переходы
startTransition(() => {
  // Переход: фильтрация списка по введенному ключевому слову
  setSearchQuery(input);
});
```
- **useTransition** - Помимо startTransition появился новый хук useTransition. Он позволяет узнать статус перехода
```
import { useTransition } from 'react';
const [isPending, startTransition] = useTransition();
```

- **useDeferredValue** позволяет отложить повторный рендеринг несрочной части дерева. Он похож на debouncing, но имеет несколько преимуществ по сравнению с ним. Фиксированной задержки по времени нет, поэтому React попытается выполнить отложенный рендеринг сразу после того, как первый рендер отобразится на экране. Отложенный рендеринг может быть прерван и не будет блокировать ввод данных пользователем.

- **useSyncExternalStore** — это новый хук, который позволяет внешним хранилищам поддерживать параллельное чтение, заставляя обновления в хранилище быть синхронными. Он устраняет необходимость в useEffect при реализации подписок на внешние источники данных и рекомендуется для любой библиотеки, которая интегрируется со сторонним состоянием по отношению к React.

- **useInsertionEffect** — это новый хук, который позволяет библиотекам CSS-in-JS решать проблемы с производительностью при внедрении стилей во время рендеринга. Этот хук запустится после изменения DOM, но до того, как эффекты лейаута узнают об этом.

### React DOM Client

- **createRoot**: новый метод создания корня для рендеринга или анмаунта. Без него новые функции в React 18 не работают
- **hydrateRoot**: новый метод гидратации приложения, отображаемого на сервере.

И **createRoot**, и **hydrateRoot** принимают новый параметр onRecoverableError, на случай, если вы хотите получать уведомления, когда React восстанавливается после ошибок во время рендеринга или гидратации для ведения журнала. По умолчанию React будет использовать reportError или console.error в старых браузерах.


ссылка: 
- https://habr.com/ru/post/660333/
- https://tproger.ru/articles/react-18-novye-huki-i-kak-izmenilsja-rendering/#:~:text=React%2018%20%D0%B4%D0%BE%D0%B1%D0%B0%D0%B2%D0%BB%D1%8F%D0%B5%D1%82%20%D0%B2%D0%BE%D0%B7%D0%BC%D0%BE%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C%20%D0%B0%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B3%D0%BE,%D1%81%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B9%20%D0%B2%20%D0%BE%D0%B4%D0%B8%D0%BD%20%D1%8D%D1%82%D0%B0%D0%BF%20%D1%80%D0%B5%D1%80%D0%B5%D0%BD%D0%B4%D0%B5%D1%80%D0%B0.