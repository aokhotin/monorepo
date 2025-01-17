# verdaccio-aws-s3-storage

📦 AWS S3 storage plugin for verdaccio

[![verdaccio (latest)](https://img.shields.io/npm/v/verdaccio-aws-s3-storage/latest.svg)](https://www.npmjs.com/package/verdaccio-aws-s3-storage)
[![CircleCI](https://circleci.com/gh/verdaccio/verdaccio-aws-s3-storage/tree/master.svg?style=svg)](https://circleci.com/gh/verdaccio/verdaccio-aws-s3-storage/tree/master)
[![Known Vulnerabilities](https://snyk.io/test/github/verdaccio/verdaccio-aws-s3-storage/badge.svg?targetFile=package.json)](https://snyk.io/test/github/verdaccio/verdaccio-aws-s3-storage?targetFile=package.json)
[![codecov](https://codecov.io/gh/verdaccio/verdaccio-aws-s3-storage/branch/master/graph/badge.svg)](https://codecov.io/gh/verdaccio/verdaccio-aws-s3-storage)
[![backers](https://opencollective.com/verdaccio/tiers/backer/badge.svg?label=Backer&color=brightgreen)](https://opencollective.com/verdaccio)
[![discord](https://img.shields.io/discord/388674437219745793.svg)](http://chat.verdaccio.org/)
![MIT](https://img.shields.io/github/license/mashape/apistatus.svg)
[![node](https://img.shields.io/node/v/verdaccio-aws-s3-storage/latest.svg)](https://www.npmjs.com/package/verdaccio-aws-s3-storage)


Based on [`verdaccio-s3-storage`](https://github.com/Remitly/verdaccio-s3-storage) built in Typescript + other features.

🚧 Alpha testing

**See it in action** in our [Docker + LocalStack + Verdaccio 4 + S3 Plugin example](https://github.com/verdaccio/docker-examples/tree/master/amazon-s3-docker-example).

### Requirements

* AWS Account
* Verdaccio server (4.0) (for 3.x use `verdaccio-s3-storage` instead)

```
npm install -g verdaccio
```

> This plugin is not supported in the version `2.x`

## Usage

```
npm install verdaccio-aws-s3-storage
```

This will pull AWS credentials from your environment.

In your verdaccio config, configure

```yaml
store:
  aws-s3-storage:
    bucket: your-s3-bucket
    keyPrefix: some-prefix # optional, has the effect of nesting all files in a subdirectory
    region: us-west-2 # optional, will use aws s3's default behavior if not specified
    endpoint: https://{service}.{region}.amazonaws.com # optional, will use aws s3's default behavior if not specified
    s3ForcePathStyle: false # optional, will use path style URLs for S3 objects
    accessKeyId: your-access-key-id # optional, aws accessKeyId for private S3 bucket
    secretAccessKey: your-secret-access-key # optional, aws secretAccessKey for private S3 bucket
```

store properties can be defined for packages. The storage location corresponds to the folder in s3 bucket.

```
packages:
  '@scope/*':
    access: all
    publish: $all
    storage: 'scoped'
  '**':
    access: $all
    publish: $all
    proxy: npmjs
    storage: 'public'
```

# Developer Testing #

In case of local testing, this project can be used self-efficiently. Four main ingredients are as follows:

* config.yaml, see [verdaccio documentation](https://verdaccio.org/docs/en/configuration.html)
* The provided docker file allows to test the plugin, with no need for main verdaccio application
* The provided docker-compose also provides minio in orchestration as a local substitute for S3 backend
* registry.envs set as follows. This file does not exist on the repo and should be generated after cloning the project.  

```    
AWS_ACCESS_KEY_ID=foobar
AWS_SECRET_ACCESS_KEY=1234567e
AWS_DEFAULT_REGION=eu-central-1
AWS_S3_ENDPOINT=https://localhost:9000/
AWS_S3_PATH_STYLE=true
```

The default values should work out of the box. If you change anything, make sure the corresponding variables are set in
other parts of the ingredient as well.
