# buf LSP: go-to-definition on an extend block's extendee finds nothing

A minimal reproduction for buf's language server (`buf lsp serve`).

`opts.proto` declares a message `Base` with an extension range, and three
extend blocks: one of `google.protobuf.FileOptions`, one of `Base`, and one
of `Base` nested in a message `Holder`.

`buf build` accepts the module.

## Steps

1. Open `opts.proto` in an editor connected to `buf lsp serve`, run from
   this directory.
2. Go to definition on `FileOptions` in `extend google.protobuf.FileOptions {`
   (line 11): no location.
3. Go to definition on `Base` in `extend Base {` (line 15), and in the
   nested `extend Base {` (line 20): no location.
4. Find references on `Base` in `message Base {` (line 7): no location.

Expected: steps 2 and 3 reach `message FileOptions` in
`google/protobuf/descriptor.proto` and `message Base` (line 7); step 4 finds
the two extend blocks of `Base`.

Seen with buf 1.73.0 and with buf `main` at `fe4085d` (1.75.1-dev).
