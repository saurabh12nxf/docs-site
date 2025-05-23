# zos command

**[zos](../zos-article.md) > [command](command-article.md)**

Interact with z/OS command related services, including z/OSMF Console services, etc.

## Usage

`zos command issue console [commandString] --console-name | --cn <consoleName> --system-name | --sn <systemName>`

## Action

- [`issue`](./issue/issue-article.md)

## Positional Arguments

- [zos command issue console](./issue/zos-command-issue-console.md#positional-arguments)

    - `commandString`

## Options

- [zos command issue console](./issue/zos-command-issue-console.md#options)

    | Full name  | Alias| Type |
    | :---- | :---- | :---- |
    | --console-name |--cn| string |
    | --system-name  |--sn|string |

## Examples

```
@bot zos command issue console “d a,l”
```
- Issue a simple command.

```
@bot zos command issue console "d a,l" --console-name test
```
- Issue a z/OS console command with a console name.
