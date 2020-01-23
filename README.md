<p align="center">
  <a href="http://nestjs.com"><img src="https://nestjs.com/img/logo_text.svg" alt="Nest Logo" width="320" /></a>
</p>

<p align="center">
  A <a href="https://github.com/nestjs/nest">Nest</a> interceptor to display the <a href="https://tools.ietf.org/html/rfc5988">Link</a> in the response headers.
</p>

A simple NestJS interceptor catching `page` and `per_page` query parameters and format a Link Header, based on [GitHub](https://developer.github.com/v3/guides/traversing-with-pagination/) pagination API.
This module uses [format-link-header](https://github.com/jonathansamines/format-link-header) node module to build the response Link Header.

## Installation

```bash
npm install --save @algoan/nestjs-link-header
```

## Limits

- On this version, the API attached with this interceptor needs to return an object:

```json
{
  "totalDocs": 1530,
  "resource": [ { ... }, ..., { ... }]
}
```

## Quick Start

Import `LinkHeaderInterceptor` next to a controller method. 

```typescript
import { LinkHeaderInterceptor } from '@algoan/nestjs-link-header';
import { Controller, Get, UserInterceptors } from '@nestjs/common';

@Controller()
/**
 * Controller returning a lot of documents
 */
class AppController {
  /**
   * Find all documents
   */
  @UseInterceptors(LinkHeaderInterceptor)
  @Get('/data')
  public async findAll(): Promise<{ totalDocs: number; resource: DataToReturn[] }> {
    const data: DataToReturn = await model.find(...);
    const count: number = await model.count()

    return { totalDocs: count, resource: data };
  }
}
```