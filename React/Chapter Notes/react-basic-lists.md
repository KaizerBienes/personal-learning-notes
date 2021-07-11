# Rendering lists and Conditional Contents
- to prevent hardcoding, we can use `map`

```js
const Example = (props) => {
    return (
        {props.expenseItems.map((expense) => {
            return <ExpenseItem
                key={expense.id}
                title={expense.title}
                amount={expense.amount}
                date={expense.date}
            />
        })}
    );
}
```
- need to add `key` so that React can properly identify an element

### Conditional Content
- adding conditional content inside JSX
```js
const Example = ((props) => {
    return (
        {filteredList.length === 0 ? <p>Nothing found!</p> : props.filteredList}
    );
});
```

### Adding styles
- inline to make it dynamic
- css properties with dashes should be camelCase, `backgroundColor` instead of `background-color`
```js
const Example = ((props) => {
    return (
        <div className="chart-bar__fill" style={{height: barFillHeight}}></div>
    );
});
```