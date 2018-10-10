Fork of the official Clojurescript node repl. Allows the nodejs process to be started separately from the Clojurescript repl, so that it can be run on a separate host.
May be useful for working on ARM-based computers (like the Raspberry Pi), where running a Clojure repl would be heavyweight, compared to just running a node process.

How I'm using this right now:
1. Run the node script with `node --eval $(cat node_repl.js)`. Passing the code in via stdin like that is important for some reason I don't really understand, having to do with the context of code being run using the vm module.
2. Start an nrepl server in one clojure process with something like `(require 'cider-nrepl.main) (cider-nrepl.main/init ["cider.nrepl/cider-middleware" "cider.piggieback/wrap-cljs-repl"])`
3. Connect editor to that nrepl port using CIDER
4. In the editor, eval `(require 'cider.piggieback) (require 'cljs-remote-node-repl) (cider.piggieback/cljs-repl (cljs-remote-node-repl/repl-env :port 5001))`

So far this is very experimental, and I've actually only tested running the node process on localhost.
