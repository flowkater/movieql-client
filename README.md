# MovieQL-Client

```shell
yarn add react-router-dom apollo-boost react-apollo graphql-tag graphql
```

- Apollo Boost
  - GraphQL 클라이언트를 가지기 위해 모든걸 대신 셋업 (추후에 커스터마이징 해야하나 여기서는 간단히 쓰기에 좋다.)

- src/apolloClient.js

```javascript
import ApolloClient from "apollo-boost";

const client = new ApolloClient({
  uri: "http://127.0.0.1:4000/"
});

export default client;
```

- src/App.js

```javascript
import React, { Component } from 'react';
import { ApolloProvider } from "react-apollo";
import client from "./apolloClient";

class App extends Component {
  render() {
    return (
      <ApolloProvider client={client}>
        <div className="App" />
      </ApolloProvider>
    );
  }
}

export default App;
```

- Apollo Dev Tools 크롬 확장을 이용해서 리액트 애플리케이션에서 Query 확인 가능

- queries.js

```javascript
import gql from "graphql-tag";

export const HOME_PAGE = gql`
  {
    movies(limit: 50, rating: 7) {
      id
      title
      genres
      rating
    }
  }
`;
```

- Home.js

```javascript
import React from 'react';
import { Query } from "react-apollo";
import { HOME_PAGE } from './queries';

const Home = () => (
  <Query query={HOME_PAGE}>
    {({ loading, data, error }) => {
      if (loading) return <span>loading</span>;
      if (error) return <span>something happened</span>;
      if (data) {
        return data.movies.map(movie => (
          <h2 key={movie.id}>
            {movie.title} / {movie.rating}
          </h2>
        ));
      }
    }}
  </Query>
);

export default Home;
```

> redux 로 관리할 필요없이 캐시 처리를 해준다. fetch, waiting 등 부가적인 코드가 필요없이 Query 라는 태그안에서 모두 처리해줌. 개편리!