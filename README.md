# NestJS S3

<a href="https://www.npmjs.com/package/nestjs-s3-aws"><img src="https://img.shields.io/npm/v/nestjs-s3-aws.svg" alt="NPM Version" /></a>
<a href="https://www.npmjs.com/package/nestjs-s3-aws"><img src="https://img.shields.io/npm/l/nestjs-s3-aws.svg" alt="Package License" /></a>

## Table of Contents

- [Description](#description)
- [Installation](#installation)
- [Examples](#examples)
- [License](#license)

## Description
Integrates S3 with Nest

## Installation

```bash
npm install nestjs-s3-aws @aws-sdk/client-s3
```

You can also use the interactive CLI

```sh
npx nestjs-modules
```

### S3Module.forRoot(options, connection?)

```ts
import { Module } from '@nestjs/common';
import { S3Module } from 'nestjs-s3-aws';
import { AppController } from './app.controller';

@Module({
  imports: [
    S3Module.forRoot({
      config: {
        credentials: {
          accessKeyId: 'minio',
          secretAccessKey: 'password',
        },
        // region: 'us-east-1',
        endpoint: 'http://127.0.0.1:9000',
        forcePathStyle: true,
        signatureVersion: 'v4',
      },
    }),
  ],
  controllers: [AppController],
})
export class AppModule {}
```

### S3Module.forRootAsync(options, connection?)

```ts
import { Module } from '@nestjs/common';
import { S3Module } from 'nestjs-s3-aws';
import { AppController } from './app.controller';

@Module({
  imports: [
    S3Module.forRootAsync({
      useFactory: () => ({
        config: {
          credentials: {
            accessKeyId: 'minio',
            secretAccessKey: 'password',
          },
          // region: 'us-east-1',
          endpoint: 'http://localhost:9000',
          forcePathStyle: true,
          signatureVersion: 'v4',
        },
      }),
    }),
  ],
  controllers: [AppController],
})
export class AppModule {}
```

### InjectS3(connection?)

```ts
import { Controller, Get, } from '@nestjs/common';
import { InjectS3, S3 } from 'nestjs-s3-aws';

@Controller()
export class AppController {
  constructor(
    @InjectS3() private readonly s3: S3,
  ) {}

  @Get()
  async listBuckets() {
    try {
      await this.s3.createBucket({ Bucket: 'bucket' });
    } catch (e) {}

    try {
      // this.s3.send(new ListBucketsCommand({}))
      const list = await this.s3.listBuckets({});
      return list.Buckets;
    } catch (e) {
      console.log(e);
    }
  }
}
```

## License

MIT
