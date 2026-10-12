# Markdown Table Line Constraints

English | [日本語](./README.ja.md)

Markdown Table Line Constraints is a proposal for a syntax that lets you declare, column by column, a constraint on the number of lines in each cell of a Markdown table.

## A Simple Example

Tables in [GitHub Flavored Markdown](https://github.github.com/gfm/) (GFM) currently have no way to declare column widths, so each renderer decides how wide each column is drawn. Depending on the overall width of the table, some columns may wrap and become hard to read, as on the left of the image below. This proposal introduces a syntax that **explicitly requests single-line display** for specific columns, as on the right of the image below.

![A side-by-side comparison of a hard-to-read display and an easy-to-read display. Both show the same two-column table with Date and Content columns. On the left, the dates "October 9" and "October 10" wrap onto two lines, broken as "Octob" and "er 9", with the caption "Both elements in the Date column of the table are displayed across two lines." On the right, the same dates appear on a single line, with the caption "Both elements in the Date column of the table are displayed in a single line."](docs/en/src/comparison-easy-with-hard-to-read.en.png)

Here is how it is written:

```md
| Date | Content |
| :--< | :-- | 
| October 9 | I slept all day. |
| October 10 | I went to bed at 6 a.m. and woke up at noon. I had a meal at 3 p.m., took a nap, and woke up again at 7 p.m. My brain felt foggy, so I just sat around doing nothing for a while. At 2 a.m., I started feeling sleepy and went to bed. | 
```

A column with `<` in its delimiter row cell, as in `:--<`, requests that its lines not wrap. Unless an unavoidable constraint such as the viewport width or the table width prevents it, a cell that fits on one line is displayed on one line.

## Current Status

In the table below, `✅` means done, `🚧` means in progress, and `N/A` means not started.

|  | Description | Specification | Parser implementation | Renderer implementation | Details |
| --: | :-- | :--: | :--: | :--: | :-- |
| v1.0 | Requesting no line wrapping with `<` | 🚧 | N/A | N/A | [docs/v1-0](docs/en/v1-0.md) |
| v1.1 | Requesting a maximum number of lines with `<n` | N/A | N/A | N/A | N/A |

In the future, I would like to support any number of lines, such as `<2` or `<5`, not just `<`.

## Repository Structure

Details about each version are in [`docs/en/`](docs/en/). The specification for each version is in [`spec/`](spec/). The details of the parser and renderer implementations are not decided yet.

## History

There has been a request for a syntax that adjusts table column widths, for example by the number of hyphens (`-`) in the delimiter row[^1]. In that discussion, commenters pointed out that widths depend on the viewport and the size of the table, which the parser and renderer cannot know, and that the feature should be implemented as an extension. The issue has since been closed.

## Philosophy

Markdown is a markup language, so it is a fair point that a document's content should be written semantically and that Markdown should not interfere with how the content is rendered or looks more than necessary. However, the GFM delimiter row, for example, already carries alignment information such as `:--:`, which is information about appearance. Besides, nobody wants to read a hard-to-read table. I believe this proposal can have a large impact on readability with an extremely simple addition to the syntax.

This proposal started from a personal experience: I came across a table like the one in [# A Simple Example](#a-simple-example) on an article-sharing site. The key point of the solution is that the syntax deliberately **does not specify column widths directly**. If you picture a rich table editor such as Microsoft Excel, a table that shows each date on two lines makes you want to widen the date column directly. However, a Markdown parser does not know what editor the writer uses or what viewer the reader uses. The font may be extremely large, and the width a column needs may vary by language. To handle such situations, I concluded that specifying "how much this column should ideally wrap", instead of "specifying the visual width directly", can adapt to any situation.

In this way, I want to offer a small idea that is easy on writers, on parsers and renderers, and on readers.

## Known Limitations and Issues

### Forward compatibility cannot be guaranteed

Backward compatibility can be guaranteed: tables that do not use `<` look the same as before. However, GFM parsers that do not support this proposal do not interpret tables with `<` as tables at all.

### Some parsers and renderers may not need this syntax

As of October 12, 2026, VS Code, GitHub, Qiita, and other tools may not display such tables as expected. However, some viewers display a table like the one in [# A Simple Example](#a-simple-example) as expected even without `<`. For example, the Markdown Preview built into the Zed text editor tries to keep a column on a single line whenever it fits.

![A side-by-side comparison of Markdown table previews in VS Code and Zed. On the left, VS Code wraps the dates "October 9" and "October 10" in the Date column onto two lines, with the caption "Both elements in the Date column of the table are displayed across two lines." On the right, Zed shows the same dates on a single line, with the caption "Both elements in the Date column of the table are displayed in a single line."](docs/en/src/comparison-vscode-with-zed.en.png)

Still, this proposal is meaningful as a way to reflect the writer's intent instead of leaving the readability of tables to the renderer. Also, when the display is even narrower, Zed also has to wrap some column, and this syntax lets the writer say which columns should take priority.

## License

MIT. See [LICENSE](./LICENSE) for details.

## References

- [**GitHub Flavored Markdown は何であって何でないか** (What GitHub Flavored Markdown is and is not, in Japanese) - Qiita](https://qiita.com/tk0miya/items/6b81e0e4563199037018)

[^1]: https://github.com/markedjs/marked/issues/266
