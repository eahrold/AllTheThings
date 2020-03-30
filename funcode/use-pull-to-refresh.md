---
layout: layout.njk
title: Use Pull to Refresh
date: 2020-03-26T17:15:32.261Z
---

### Here's a fun RN hook for a Pull to Refresh ability

So one of the things that I never liked about the RefreshControl, refreshing/onRefresh
was that most of the time onRefresh was actually a direct function where you had do do
a bunch of state management with the refreshing state.

It's one place that always felt more imperative than it did reactive.
So this hook provides a property a load `useEffect` hook can respond to.

```js
// usePullToRefresh.js
const usePullToRefresh() => {
    const [refreshing, setRefreshing] = useState(false)
    const [refreshedOn, setRefreshedOn] = useState(Date.now())

    const onRefresh = useCallback(() => {
        setRefreshing(true)
        setRefreshedOn(Date.now())
    }, [])

    return {refreshing, onRefresh, setRefreshing, refreshedOn}
}

```

The Cool thing about this is that it cleanly keeps you loading reactive, so you're not calling `load()` you're watching when to load.

Since setRefreshing is a useState setter, you can safely include it
inside of your dependency array without re-triggering the `load()` call.

```js
const [data, setData] = useState([])
const { refreshing, refreshedOn, onRefresh, setRefresh} = usePullToRefresh()

useEffect(()=>{
    const load = async () => {
        const data = await loadSomeData()
        setData(data)
        setRefreshing(false)
    }
    load()
}, [refreshedOn, setRefreshing, otherProps])
return <FlatList
        data={data}
        refreshing={refreshing}
        onRefresh={onRefresh}
        ...
    >
```

Another benefit of this is the same load useEffect is triggered on mount, so a separate one isn't needed to uniquely handle a refreshing state.

