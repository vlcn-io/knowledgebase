```
import "./styles.css";
import {useCallback, useState} from "react"

export default function App() {
  const [value, setValue] = useState('');
  const [taskValue, setTaskValue] = useState('');
  subscriber = useCallback((v) => {
    setValue(v);
  }, []);
  taskSubscriber = useCallback((v) => {
    setTaskValue(v)
  }, []);

  return (
    <div className="App">
      Micro task input: <input type="text" value={value} onChange={(e) => {persist(e.target.value)}} />
      <br/><br/>
      Task input: <input type="text" value={taskValue} onChange={(e) => {persistWithTask(e.target.value)}} />
    </div>
  );
}

let subscriber;
let taskSubscriber;

function persist(v) {
  queueMicrotask(() => {
    subscriber(v)
  })
}

function persistWithTask(v) {
  setTimeout(() => {
    taskSubscriber(v)
  }, 0);
}
```

https://github.com/facebook/react/issues/24365
https://github.com/facebook/react/issues/24625