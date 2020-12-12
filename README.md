# S3 Clipboard

![shellcheck](https://github.com/kljensen/s3clipboard/workflows/shellcheck/badge.svg?event=push)

**Table Of Contents**
- [Project description](#project-description)
- [Installation](#installation)
- [Example Usage](#example-usage)
- [Contributing](#contributing)
- [Change log](#change-log)
- [License](#license)

#### Project description:

`s3clipboard` is a small, Posix-compliant shell script for using S3 as a
clipboard register. This lets you "copy" from one machine and "paste" on
another.  You can optionally encrypt the contents of the clipboard with a
symmetric cipher using openssl. This project is similar to
[sqs_clipboard](https://github.com/jftuga/sqs_clipboard) but it uses S3 instead
of SQS and it is shell script instead of a Go binary.

## Installation

You can either install this project by checking out the git repo

```
git clone https://github.com/kljensen/s3clipboard.git
```

Or by downloading the `s3clipboard` shell script directly

```
curl -O https://raw.githubusercontent.com/kljensen/s3clipboard/main/s3clipboard
```

**Requirements/Dependencies:**
- The [AWS CLI](https://github.com/aws/aws-cli)
- [OpenSSL](https://www.openssl.org/) or [LibreSSL](https://www.libressl.org/) for encryption
- A Posix-compliant shell (`/bin/sh`: [the Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell) by default)

## Example Usage

The shell script has two commands: `copy` and `paste`. The `copy` command
will read stdin and write it to an object on S3. The paste command will
read an object on S3 and write it to stdout.

```
Usage: ./s3clipboard [-e] [-p PASSWORD] [-b BUCKET] [-o OBJECT_KEY] (copy|paste)
```

The `-e` flag turns on encryption, which requires an OpenSSL implementation.
You may optionally specify a password with `-p`. If you specify `-e` without
`-p` you will be prompted for a password. The `-a` allows you to select a 
named AWS profile.

You can specify your destination bucket on S3 using the `-b` flag and your
destination object within that bucket with the `-o` flag.

The script also reads a number of environment variables.

| Environment Variable | Meaning |
| --------------- | --------------- |
| `S3CLIPBOARD_ENCRYPT` | Non-empty value or `-e` turns on encryption. | 
| `S3CLIPBOARD_PASSWORD` | Non-empty value or `-p` argument sets encryption password. | 
| `S3CLIPBOARD_BUCKET` | Non-empty value or `-b` argument sets S3 bucket. | 
| `S3CLIPBOARD_OBJECT` | Non-empty value or `-o` argument sets the S3 object within the bucket. | 
| `S3CLIPBOARD_AWS_PROFILE` | Non-empty value or `-a` argument sets the named AWS profile. | 

The shell session below shows copying and pasting to and from S3.

```
> export S3CLIPBOARD_BUCKET="my-favorite-bucket"
> export S3CLIPBOARD_ENCRYPT="true"
> export S3CLIPBOARD_PASSWORD="my-secret-pass"
> echo "my secret content" | ./s3clipboard copy
> ./s3clipboard paste
my secret content
```

## Contributing

All issues and pull requests are welcome. I ask only that you run
[ShellCheck](https://github.com/koalaman/shellcheck) before submitting a pull
request.

## Change Log
[CHANGELOG.md](./CHANGELOG.md)

## License

See the [LICENSE](LICENSE) file.
