# cargo-check(1)


## NAME

cargo-check - Check the current package

## SYNOPSIS

`cargo check` [_options_]

## DESCRIPTION

Check a local package and all of its dependencies for errors. This will
essentially compile the packages without performing the final step of code
generation, which is faster than running `cargo build`. The compiler will save
metadata files to disk so that future runs will reuse them if the source has
not been modified. Some diagnostics and errors are only emitted during code
generation, so they inherently won't be reported with `cargo check`.

## OPTIONS

### Package Selection

By default, when no package selection options are given, the packages selected
depend on the selected manifest file (based on the current working directory if
`--manifest-path` is not given). If the manifest is the root of a workspace then
the workspaces default members are selected, otherwise only the package defined
by the manifest will be selected.

The default members of a workspace can be set explicitly with the
`workspace.default-members` key in the root manifest. If this is not set, a
virtual workspace will include all workspace members (equivalent to passing
`--workspace`), and a non-virtual workspace will include only the root crate itself.

<dl>

<dt class="option-term" id="option-cargo-check--p"><a class="option-anchor" href="#option-cargo-check--p"></a><code>-p</code> <em>spec</em>...</dt>
<dt class="option-term" id="option-cargo-check---package"><a class="option-anchor" href="#option-cargo-check---package"></a><code>--package</code> <em>spec</em>...</dt>
<dd class="option-desc">Check only the specified packages. See <a href="cargo-pkgid.html">cargo-pkgid(1)</a> for the
SPEC format. This flag may be specified multiple times and supports common Unix
glob patterns like <code>*</code>, <code>?</code> and <code>[]</code>. However, to avoid your shell accidentally 
expanding glob patterns before Cargo handles them, you must use single quotes or
double quotes around each pattern.</dd>


<dt class="option-term" id="option-cargo-check---workspace"><a class="option-anchor" href="#option-cargo-check---workspace"></a><code>--workspace</code></dt>
<dd class="option-desc">Check all members in the workspace.</dd>



<dt class="option-term" id="option-cargo-check---all"><a class="option-anchor" href="#option-cargo-check---all"></a><code>--all</code></dt>
<dd class="option-desc">Deprecated alias for <code>--workspace</code>.</dd>



<dt class="option-term" id="option-cargo-check---exclude"><a class="option-anchor" href="#option-cargo-check---exclude"></a><code>--exclude</code> <em>SPEC</em>...</dt>
<dd class="option-desc">Exclude the specified packages. Must be used in conjunction with the
<code>--workspace</code> flag. This flag may be specified multiple times and supports
common Unix glob patterns like <code>*</code>, <code>?</code> and <code>[]</code>. However, to avoid your shell
accidentally expanding glob patterns before Cargo handles them, you must use
single quotes or double quotes around each pattern.</dd>


</dl>


### Target Selection

When no target selection options are given, `cargo check` will check all
binary and library targets of the selected packages. Binaries are skipped if
they have `required-features` that are missing.

Passing target selection flags will check only the specified
targets. 

Note that `--bin`, `--example`, `--test` and `--bench` flags also 
support common Unix glob patterns like `*`, `?` and `[]`. However, to avoid your 
shell accidentally expanding glob patterns before Cargo handles them, you must 
use single quotes or double quotes around each glob pattern.

<dl>

<dt class="option-term" id="option-cargo-check---lib"><a class="option-anchor" href="#option-cargo-check---lib"></a><code>--lib</code></dt>
<dd class="option-desc">Check the package's library.</dd>


<dt class="option-term" id="option-cargo-check---bin"><a class="option-anchor" href="#option-cargo-check---bin"></a><code>--bin</code> <em>name</em>...</dt>
<dd class="option-desc">Check the specified binary. This flag may be specified multiple times
and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-check---bins"><a class="option-anchor" href="#option-cargo-check---bins"></a><code>--bins</code></dt>
<dd class="option-desc">Check all binary targets.</dd>



<dt class="option-term" id="option-cargo-check---example"><a class="option-anchor" href="#option-cargo-check---example"></a><code>--example</code> <em>name</em>...</dt>
<dd class="option-desc">Check the specified example. This flag may be specified multiple times
and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-check---examples"><a class="option-anchor" href="#option-cargo-check---examples"></a><code>--examples</code></dt>
<dd class="option-desc">Check all example targets.</dd>


<dt class="option-term" id="option-cargo-check---test"><a class="option-anchor" href="#option-cargo-check---test"></a><code>--test</code> <em>name</em>...</dt>
<dd class="option-desc">Check the specified integration test. This flag may be specified
multiple times and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-check---tests"><a class="option-anchor" href="#option-cargo-check---tests"></a><code>--tests</code></dt>
<dd class="option-desc">Check all targets in test mode that have the <code>test = true</code> manifest
flag set. By default this includes the library and binaries built as
unittests, and integration tests. Be aware that this will also build any
required dependencies, so the lib target may be built twice (once as a
unittest, and once as a dependency for binaries, integration tests, etc.).
Targets may be enabled or disabled by setting the <code>test</code> flag in the
manifest settings for the target.</dd>


<dt class="option-term" id="option-cargo-check---bench"><a class="option-anchor" href="#option-cargo-check---bench"></a><code>--bench</code> <em>name</em>...</dt>
<dd class="option-desc">Check the specified benchmark. This flag may be specified multiple
times and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-check---benches"><a class="option-anchor" href="#option-cargo-check---benches"></a><code>--benches</code></dt>
<dd class="option-desc">Check all targets in benchmark mode that have the <code>bench = true</code>
manifest flag set. By default this includes the library and binaries built
as benchmarks, and bench targets. Be aware that this will also build any
required dependencies, so the lib target may be built twice (once as a
benchmark, and once as a dependency for binaries, benchmarks, etc.).
Targets may be enabled or disabled by setting the <code>bench</code> flag in the
manifest settings for the target.</dd>


<dt class="option-term" id="option-cargo-check---all-targets"><a class="option-anchor" href="#option-cargo-check---all-targets"></a><code>--all-targets</code></dt>
<dd class="option-desc">Check all targets. This is equivalent to specifying <code>--lib --bins --tests --benches --examples</code>.</dd>


</dl>


### Feature Selection

The feature flags allow you to control which features are enabled. When no
feature options are given, the `default` feature is activated for every
selected package.

See [the features documentation](../reference/features.html#command-line-feature-options)
for more details.

<dl>

<dt class="option-term" id="option-cargo-check---features"><a class="option-anchor" href="#option-cargo-check---features"></a><code>--features</code> <em>features</em></dt>
<dd class="option-desc">Space or comma separated list of features to activate. Features of workspace
members may be enabled with <code>package-name/feature-name</code> syntax. This flag may
be specified multiple times, which enables all specified features.</dd>


<dt class="option-term" id="option-cargo-check---all-features"><a class="option-anchor" href="#option-cargo-check---all-features"></a><code>--all-features</code></dt>
<dd class="option-desc">Activate all available features of all selected packages.</dd>


<dt class="option-term" id="option-cargo-check---no-default-features"><a class="option-anchor" href="#option-cargo-check---no-default-features"></a><code>--no-default-features</code></dt>
<dd class="option-desc">Do not activate the <code>default</code> feature of the selected packages.</dd>


</dl>


### Compilation Options

<dl>

<dt class="option-term" id="option-cargo-check---target"><a class="option-anchor" href="#option-cargo-check---target"></a><code>--target</code> <em>triple</em></dt>
<dd class="option-desc">Check for the given architecture. The default is the host
architecture. The general format of the triple is
<code>&lt;arch&gt;&lt;sub&gt;-&lt;vendor&gt;-&lt;sys&gt;-&lt;abi&gt;</code>. Run <code>rustc --print target-list</code> for a
list of supported targets.</p>
<p>This may also be specified with the <code>build.target</code>
<a href="../reference/config.html">config value</a>.</p>
<p>Note that specifying this flag makes Cargo run in a different mode where the
target artifacts are placed in a separate directory. See the
<a href="../guide/build-cache.html">build cache</a> documentation for more details.</dd>



<dt class="option-term" id="option-cargo-check---release"><a class="option-anchor" href="#option-cargo-check---release"></a><code>--release</code></dt>
<dd class="option-desc">Check optimized artifacts with the <code>release</code> profile.
See also the <code>--profile</code> option for choosing a specific profile by name.</dd>



<dt class="option-term" id="option-cargo-check---profile"><a class="option-anchor" href="#option-cargo-check---profile"></a><code>--profile</code> <em>name</em></dt>
<dd class="option-desc">Check with the given profile.</p>
<p>As a special case, specifying the <code>test</code> profile will also enable checking in
test mode which will enable checking tests and enable the <code>test</code> cfg option.
See <a href="https://doc.rust-lang.org/rustc/tests/index.html">rustc tests</a> for more
detail.</p>
<p>See the <a href="../reference/profiles.html">the reference</a> for more details on profiles.</dd>



<dt class="option-term" id="option-cargo-check---ignore-rust-version"><a class="option-anchor" href="#option-cargo-check---ignore-rust-version"></a><code>--ignore-rust-version</code></dt>
<dd class="option-desc">Check the target even if the selected Rust compiler is older than the
required Rust version as configured in the project's <code>rust-version</code> field.</dd>



</dl>

### Output Options

<dl>
<dt class="option-term" id="option-cargo-check---target-dir"><a class="option-anchor" href="#option-cargo-check---target-dir"></a><code>--target-dir</code> <em>directory</em></dt>
<dd class="option-desc">Directory for all generated artifacts and intermediate files. May also be
specified with the <code>CARGO_TARGET_DIR</code> environment variable, or the
<code>build.target-dir</code> <a href="../reference/config.html">config value</a>.
Defaults to <code>target</code> in the root of the workspace.</dd>


</dl>

### Display Options

<dl>
<dt class="option-term" id="option-cargo-check--v"><a class="option-anchor" href="#option-cargo-check--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-check---verbose"><a class="option-anchor" href="#option-cargo-check---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">Use verbose output. May be specified twice for &quot;very verbose&quot; output which
includes extra output such as dependency warnings and build script output.
May also be specified with the <code>term.verbose</code>
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-check--q"><a class="option-anchor" href="#option-cargo-check--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-check---quiet"><a class="option-anchor" href="#option-cargo-check---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">No output printed to stdout.</dd>


<dt class="option-term" id="option-cargo-check---color"><a class="option-anchor" href="#option-cargo-check---color"></a><code>--color</code> <em>when</em></dt>
<dd class="option-desc">Control when colored output is used. Valid values:</p>
<ul>
<li><code>auto</code> (default): Automatically detect if color support is available on the
terminal.</li>
<li><code>always</code>: Always display colors.</li>
<li><code>never</code>: Never display colors.</li>
</ul>
<p>May also be specified with the <code>term.color</code>
<a href="../reference/config.html">config value</a>.</dd>



<dt class="option-term" id="option-cargo-check---message-format"><a class="option-anchor" href="#option-cargo-check---message-format"></a><code>--message-format</code> <em>fmt</em></dt>
<dd class="option-desc">The output format for diagnostic messages. Can be specified multiple times
and consists of comma-separated values. Valid values:</p>
<ul>
<li><code>human</code> (default): Display in a human-readable text format. Conflicts with
<code>short</code> and <code>json</code>.</li>
<li><code>short</code>: Emit shorter, human-readable text messages. Conflicts with <code>human</code>
and <code>json</code>.</li>
<li><code>json</code>: Emit JSON messages to stdout. See
<a href="../reference/external-tools.html#json-messages">the reference</a>
for more details. Conflicts with <code>human</code> and <code>short</code>.</li>
<li><code>json-diagnostic-short</code>: Ensure the <code>rendered</code> field of JSON messages contains
the &quot;short&quot; rendering from rustc. Cannot be used with <code>human</code> or <code>short</code>.</li>
<li><code>json-diagnostic-rendered-ansi</code>: Ensure the <code>rendered</code> field of JSON messages
contains embedded ANSI color codes for respecting rustc's default color
scheme. Cannot be used with <code>human</code> or <code>short</code>.</li>
<li><code>json-render-diagnostics</code>: Instruct Cargo to not include rustc diagnostics in
in JSON messages printed, but instead Cargo itself should render the
JSON diagnostics coming from rustc. Cargo's own JSON diagnostics and others
coming from rustc are still emitted. Cannot be used with <code>human</code> or <code>short</code>.</li>
</ul></dd>


</dl>

### Manifest Options

<dl>
<dt class="option-term" id="option-cargo-check---manifest-path"><a class="option-anchor" href="#option-cargo-check---manifest-path"></a><code>--manifest-path</code> <em>path</em></dt>
<dd class="option-desc">Path to the <code>Cargo.toml</code> file. By default, Cargo searches for the
<code>Cargo.toml</code> file in the current directory or any parent directory.</dd>



<dt class="option-term" id="option-cargo-check---frozen"><a class="option-anchor" href="#option-cargo-check---frozen"></a><code>--frozen</code></dt>
<dt class="option-term" id="option-cargo-check---locked"><a class="option-anchor" href="#option-cargo-check---locked"></a><code>--locked</code></dt>
<dd class="option-desc">Either of these flags requires that the <code>Cargo.lock</code> file is
up-to-date. If the lock file is missing, or it needs to be updated, Cargo will
exit with an error. The <code>--frozen</code> flag also prevents Cargo from
attempting to access the network to determine if it is out-of-date.</p>
<p>These may be used in environments where you want to assert that the
<code>Cargo.lock</code> file is up-to-date (such as a CI build) or want to avoid network
access.</dd>


<dt class="option-term" id="option-cargo-check---offline"><a class="option-anchor" href="#option-cargo-check---offline"></a><code>--offline</code></dt>
<dd class="option-desc">Prevents Cargo from accessing the network for any reason. Without this
flag, Cargo will stop with an error if it needs to access the network and
the network is not available. With this flag, Cargo will attempt to
proceed without the network if possible.</p>
<p>Beware that this may result in different dependency resolution than online
mode. Cargo will restrict itself to crates that are downloaded locally, even
if there might be a newer version as indicated in the local copy of the index.
See the <a href="cargo-fetch.html">cargo-fetch(1)</a> command to download dependencies before going
offline.</p>
<p>May also be specified with the <code>net.offline</code> <a href="../reference/config.html">config value</a>.</dd>


</dl>

### Common Options

<dl>

<dt class="option-term" id="option-cargo-check-+toolchain"><a class="option-anchor" href="#option-cargo-check-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-check--h"><a class="option-anchor" href="#option-cargo-check--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-check---help"><a class="option-anchor" href="#option-cargo-check---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-check--Z"><a class="option-anchor" href="#option-cargo-check--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


### Miscellaneous Options

<dl>
<dt class="option-term" id="option-cargo-check--j"><a class="option-anchor" href="#option-cargo-check--j"></a><code>-j</code> <em>N</em></dt>
<dt class="option-term" id="option-cargo-check---jobs"><a class="option-anchor" href="#option-cargo-check---jobs"></a><code>--jobs</code> <em>N</em></dt>
<dd class="option-desc">Number of parallel jobs to run. May also be specified with the
<code>build.jobs</code> <a href="../reference/config.html">config value</a>. Defaults to
the number of CPUs.</dd>


</dl>

## ENVIRONMENT

See [the reference](../reference/environment-variables.html) for
details on environment variables that Cargo reads.


## EXIT STATUS

* `0`: Cargo succeeded.
* `101`: Cargo failed to complete.


## EXAMPLES

1. Check the local package for errors:

       cargo check

2. Check all targets, including unit tests:

       cargo check --all-targets --profile=test

## SEE ALSO
[cargo(1)](cargo.html), [cargo-build(1)](cargo-build.html)
