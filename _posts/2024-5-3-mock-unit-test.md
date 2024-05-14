---
layout: post
title: Mocking pg and sqlite3 Library with Jest using Typescript
---

In this post, I will be explaining how to mock the pg and sqlite3 library with Jest using Typescript. If we have a simple project with file like this :

## Create a new file

Create a new file `src/index.ts` and add the following code.

```typescript title="src/index.ts"
import * as pg from "pg";
import * as sqlite3 from "sqlite3";

export const getAllUsers = async () => {
  const client = new pg.Client();
  await client.connect();
  const res = await client.query("SELECT * FROM users");
  return res.rows;
};

export const getAllUsersFromSqlite = async () => {
  const db = new sqlite3.Database(":memory:");
  const res = await db.all("SELECT * FROM users");
  return res;
};
```

## Create mocks

We will be using the [jest-mock](https://jestjs.io/docs/manual-mocks) to mock the library. 


### Create a mock file for pg

First we need to create a mock file for pg.

```typescript title="src/__mocks__/pg.ts"
const mockQuery = jest.fn();
const mockRelease = jest.fn();

class MockClient {
  query = mockQuery;
  release = mockRelease;
}

class MockPool {
  connect() {
    return Promise.resolve(new MockClient());
  }
}

module.exports = { Pool: MockPool };
```

### Create a mock file for sqlite3

And then we will create a mock file for sqlite3.

```typescript title="src/\_\_mocks\_\_/sqlite.ts"
const mockDatabaseAll = jest.fn();
const mockDatabaseRun = jest.fn();
const mockDatabaseGet = jest.fn();
const mockDatabaseClose = jest.fn();

class MockDatabase {
  all() {
    return mockDatabaseAll;
  }

  run() {
    return mockDatabaseRun;
  }

  get() {
    return mockDatabaseGet;
  }

  close() {
    return mockDatabaseClose;
  }
}

module.exports = { Database: MockDatabase };
```

## Create a test file and use the mock

Now we will create a test file and use the mock. create a new file `__tests__/index.test.ts` and add the following code.

```typescript title="src/__tests__/index.test.ts"
import { Pool } from 'pg';
import { Database } from 'sqlite3';
import { mockDatabase, mockDatabaseAll } from './mockSqlite3';
import { getAllUsers, getAllUsersFromSqlite } from '../src/index';

jest.mock('pg', () => require('../../__mocks__/pg'));
jest.mock('sqlite3', () => require('../../__mocks__/sqlite'));

describe('getAllUsers', () => {
  it('should return all users', async () => {
    const mockQueryResult = [
      {
        username: 'Muslil1',
        email: 'muslil1@gmail.com',
        password: '123456',
      },
      {
        username: 'Muslil2',
        email: 'muslil2@gmail.com',
        password: '123456789',
      },
    ];

    Pool.prototype.connect = jest.fn().mockResolvedValueOnce({
      query: jest.fn().mockResolvedValueOnce({ rows: mockQueryResult }),
      release: jest.fn(),
    });

    const users = await getAllUsers();
    expect(users).toHaveLength(2);
  });
});

describe('getAllUsersFromSqlite', () => {
  it('should retrieve all users from the database', async () => {
    // Mock the behavior of the `all` method
    const mockDatabaseAll =  { id: 1, name: 'John' }, { id: 2, name: 'Alice' };
    Database.prototype.all = jest.fn().mockImplementation((query, callback) => {
      callback(null, mockDatabaseAll);
    });

    const users = await getAllUsersFromSqlite();
    expect(users).toEqual([{ id: 1, name: 'John' }, { id: 2, name: 'Alice' }]);
  });
});
```

By using the mock, we can test the behavior of the library and make sure that the library is working as expected. We can also use the mock to test the behavior of the library in different scenarios and make sure that it is working correctly in all scenarios. Every edge case should be tested with the mock. 

## Conclusion

In this post, we have learned how to mock the pg and sqlite3 library with Jest using Typescript. We have also seen how to use the mock to test the behavior of the library and make sure that it is working as expected. By using the mock, we can test the behavior of the library in different scenarios and make sure that it is working correctly in all scenarios. Every edge case should be tested with the mock. 

I hope this post has been helpful in understanding how to mock the pg and sqlite3 library with Jest using Typescript. If you have any questions or comments, please feel free to leave them below. Thank you for reading!