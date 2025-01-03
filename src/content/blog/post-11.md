---
title: 'Giving Back by Mentoring'
publishDate: '2021-08-27'
excerpt: 'I am offering voluntary and accessible mentorship to aspiring developers'
tags:
  - Web development
seo:
  image:
    src: '/post-11.jpg'
    alt: Mountains
---

![Mountains](/post-11.jpg)

Hello, thank you for checking this blog post. In this blog, I hope to help clarify to the difference in **server state** and **client state**, and suggest a pattern to reduce engineer time and boost developer experience to managing state.

As a developer, it is important to recognize the fact that these two are inherently different problems and should be treated differently. I've made the mistake of putting every data in a state management tool as a centralized source of truth in hope to make things simpler. As you'd imagine, it's a nightmare to cover edge cases and ensure everything is in sync and up to date. **Server state should be treated as cache and grant the client with quick access**.

## Common State Types

Below is a list of common state that may exist in your application.

- Client State
  - State that's only useful for control interactive part of the UI
  - E.g: Theme, Dark mode, modals open
- Server Cache State
  - State persisted and owned by the server, and accessed by the client
  - E.g: API calls, Response data
- Component state
  - State that component (React) needs to detect changes for re-rendering
  - E.g: Values, loading
- Form State
  - Various state to validate a form
  - E.g: isPristine, isTouch, validation,
- URL State
  - State persisted by the browser
  - E.g: Query parameter, pagination, page ID
- State Machine
  - A finite-state machine refer to a mathematical model of computation

## Different State Management Tools

Here's is list of popular tools that are being used by the industry.

- [useState/Reducer + Context Hook](https://reactjs.org/docs/hooks-intro.html)
- [Redux](https://redux-form.com/7.0.3/docs/gettingstarted.md/)
- [Mobx](https://mobx.js.org/README.html)
- [xState](https://github.com/davidkpiano/xstate)
- [jotai](https://github.com/pmndrs/jotai)
- [React-Query](https://react-query.tanstack.com/)
- [SWR](https://swr.vercel.app/)
- [Apollo Client](https://www.apollographql.com/docs/react/)

## Property of App State Vs Server State

Below are some of the main difference between an client state and server state:

| App             | Server               |
| --------------- | -------------------- |
| Non-Persistent  | Remotely Persisted   |
| Synchronous     | Asynchronous         |
| Client-Owned    | Shared Ownership     |
| Reliable Update | Potentially Outdated |

## Let's Cut to the Chase!

Both [React-Query](https://react-query.tanstack.com/) and
[SWR](https://swr.vercel.app/) provide good solution. And [Apollo Client](https://www.apollographql.com/docs/react/) if you're using GraphQL. Give it a try, I believe you would be happy to use any of these.

They will not only help you remove a handful lines of complicated code and make your application more maintainable, but have a direct impact for your users because it will feel faster and more responsive.

The most beautiful thing is that there will be only 1 request sent to the API, because they use the same key and the request is deduped, cached and shared automatically.

[SWR](https://swr.vercel.app/)

```jsx
import useSWR from 'swr'

const fetcher = (...args) => fetch(...args).then((res) => res.json())

function Profile() {
  const { data, error } = useSWR('/api/user/123', fetcher)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>

  // render data
  return <div>hello {data.name}!</div>
}
```

[React-Query](https://react-query.tanstack.com/)

```jsx
import { useQuery } from 'react-query'

export default function App() {
  const { isLoading, isError, data, error } = useQuery('todos', fetchTodoList)

  if (isLoading) return <span>Loading...</span>

  if (isError) return <span>Error: {error.message}</span>

  // We can assume by this point that `isSuccess === true`, and render data
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```

## Thank You for Reading!

While state management has been nuanced in building applications at scale, I think having a good grasp of various types of state and understanding their trade-offs would help developing apps easier.

I hope you find this post useful, and if you have any comment or suggestion, feel free to tweet or make a pull request. Cheers.
