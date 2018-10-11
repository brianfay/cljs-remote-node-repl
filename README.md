Fork of the official Clojurescript node repl. Allows the nodejs process to be started separately from the Clojurescript repl, so that it can be run on a separate host.
May be useful for working on ARM-based computers (like the Raspberry Pi), where running a Clojure repl would be heavyweight, compared to just running a node process.

How I'm using this right now:
1. Run the node script with `node --eval $(cat node_repl.js)`. Passing the code in via stdin like that is important for some reason I don't really understand, having to do with the context of code being run using the vm module.
2. Start an nrepl server in one clojure process with something like `(require 'cider-nrepl.main) (cider-nrepl.main/init ["cider.nrepl/cider-middleware" "cider.piggieback/wrap-cljs-repl"])`
3. Connect editor to that nrepl port using CIDER
4. In the editor, eval `(require 'cider.piggieback) (require 'cljs-remote-node-repl) (cider.piggieback/cljs-repl (cljs-remote-node-repl/repl-env :port 5001))`

I was able to test this running the repl on my laptop, and running the node process on a Beaglebone black, but it was a bit involved - in the setup function of the repl, cljs.core gets compiled and the output normally goes to a .cljs_node_repl directory. If you want to run the node process on the remote machine, it needs this directory. So I scp'd it over to the remote Beaglebone, in the directory where I started the node process.

Then I hacked up the root-path binding in setup to reflect the directory on the Beaglebone. This time the repl succeeded in connecting.

However, the repl started using up tons of cpu on my laptop. (The remote machine seemed to be performing fine)

Sooo.... this needs some work, but it does seem feasible.
