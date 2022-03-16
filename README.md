# Reproducer for a rust ICE (internal compiler error)

```
./rustc --version --verbose
rustc 1.61.0-nightly (52b34550a 2022-03-15)
binary: rustc
commit-hash: 52b34550aca5f7dd7e152f773e3ab786acb86f6f
commit-date: 2022-03-15
host: x86_64-unknown-linux-gnu
release: 1.61.0-nightly
LLVM version: 14.0.0

```

```
$ cargo +nightly --version --verbose
cargo 1.61.0-nightly (65c8266 2022-03-09)
release: 1.61.0-nightly
commit-hash: 65c82664263feddc5fe2d424be0993c28d46377a
commit-date: 2022-03-09
host: x86_64-unknown-linux-gnu
libgit2: 1.4.1 (sys:0.14.1 vendored)
libcurl: 7.80.0-DEV (sys:0.4.51+curl-7.80.0 vendored ssl:OpenSSL/1.1.1m)
os: Arch Linux Rolling Release [64-bit]
```

## How to reproduce:

`cargo +nightly check`

## Error message


```
thread 'rustc' panicked at 'assertion failed: !value.has_escaping_bound_vars()', /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/compiler/rustc_middle/src/ty/sty.rs:1089:9
stack backtrace:
0:     0x7f770fa9d9bd - std::backtrace_rs::backtrace::libunwind::trace::h4630fb3ea0978244
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/../../backtrace/src/backtrace/libunwind.rs:93:5
1:     0x7f770fa9d9bd - std::backtrace_rs::backtrace::trace_unsynchronized::hf18a5a72046346f1
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/../../backtrace/src/backtrace/mod.rs:66:5
2:     0x7f770fa9d9bd - std::sys_common::backtrace::_print_fmt::hf749d0aa30a58698
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/sys_common/backtrace.rs:66:5
3:     0x7f770fa9d9bd - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h9b8410cdaf24e94d
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/sys_common/backtrace.rs:45:22
4:     0x7f770faf749c - core::fmt::write::h4628ef2f511cb2a1
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/core/src/fmt/mod.rs:1190:17
5:     0x7f770fa8ef41 - std::io::Write::write_fmt::h0500ef668fc0f1ca
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/io/mod.rs:1655:15
6:     0x7f770faa0a35 - std::sys_common::backtrace::_print::h0bef26ee54194799
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/sys_common/backtrace.rs:48:5
7:     0x7f770faa0a35 - std::sys_common::backtrace::print::hd94733c6e7c5f533
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/sys_common/backtrace.rs:35:9
8:     0x7f770faa0a35 - std::panicking::default_hook::{{closure}}::heee50b6b48941df3
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/panicking.rs:295:22
9:     0x7f770faa06e9 - std::panicking::default_hook::hfbe8348e52a92768
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/panicking.rs:314:9
10:     0x7f77102ca291 - rustc_driver[689e96f3a05e42de]::DEFAULT_HOOK::{closure#0}::{closure#0}
11:     0x7f770faa1180 - std::panicking::rust_panic_with_hook::h123c71abd7d4640f
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/panicking.rs:702:17
12:     0x7f770faa0f79 - std::panicking::begin_panic_handler::{{closure}}::h4dff286ac3d07322
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/panicking.rs:586:13
13:     0x7f770fa9de74 - std::sys_common::backtrace::__rust_end_short_backtrace::h1350a023dcdb3181
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/sys_common/backtrace.rs:138:18
14:     0x7f770faa0ce9 - rust_begin_unwind
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/panicking.rs:584:5
15:     0x7f770fa64c73 - core::panicking::panic_fmt::hdc1c83fe453b97e2
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/core/src/panicking.rs:143:14
16:     0x7f770fa64b3d - core::panicking::panic::hb1922f21f0c33350
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/core/src/panicking.rs:48:5
17:     0x7f7711efd31f - rustc_trait_selection[ca55f1b2036c5dee]::traits::type_known_to_meet_bound_modulo_regions
18:     0x7f7711795df5 - <rustc_infer[769fe71a64f3976f]::infer::InferCtxtBuilder>::enter::<bool, rustc_ty_utils[89a13c48e2035fd7]::common_traits::is_item_raw::{closure#0}>
19:     0x7f771244c04e - rustc_ty_utils[89a13c48e2035fd7]::common_traits::is_item_raw
20:     0x7f7711c259a4 - <rustc_query_system[ccda753483a689aa]::dep_graph::graph::DepGraph<rustc_middle[f9f9a681d39429ab]::dep_graph::dep_node::DepKind>>::with_task::<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt, rustc_middle[f9f9a681d39429ab]::ty::ParamEnvAnd<rustc_middle[f9f9a681d39429ab]::ty::Ty>, bool>
21:     0x7f7711b7627d - rustc_query_system[ccda753483a689aa]::query::plumbing::try_execute_query::<rustc_query_impl[7c11a1a95cade8b5]::plumbing::QueryCtxt, rustc_query_system[ccda753483a689aa]::query::caches::DefaultCache<rustc_middle[f9f9a681d39429ab]::ty::ParamEnvAnd<rustc_middle[f9f9a681d39429ab]::ty::Ty>, bool>>
22:     0x7f771264133b - rustc_query_system[ccda753483a689aa]::query::plumbing::get_query::<rustc_query_impl[7c11a1a95cade8b5]::queries::is_sized_raw, rustc_query_impl[7c11a1a95cade8b5]::plumbing::QueryCtxt>
23:     0x7f77120a5e55 - <rustc_middle[f9f9a681d39429ab]::ty::Ty>::is_sized
24:     0x7f771202fd8e - <rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutCx<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt>>::layout_of_uncached
25:     0x7f7712046d06 - rustc_middle[f9f9a681d39429ab]::ty::layout::layout_of
26:     0x7f7711c25196 - <rustc_query_system[ccda753483a689aa]::dep_graph::graph::DepGraph<rustc_middle[f9f9a681d39429ab]::dep_graph::dep_node::DepKind>>::with_task::<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt, rustc_middle[f9f9a681d39429ab]::ty::ParamEnvAnd<rustc_middle[f9f9a681d39429ab]::ty::Ty>, core[2748ca54e7081f8b]::result::Result<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>, rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutError>>
27:     0x7f7711bd67f5 - rustc_query_system[ccda753483a689aa]::query::plumbing::get_query::<rustc_query_impl[7c11a1a95cade8b5]::queries::layout_of, rustc_query_impl[7c11a1a95cade8b5]::plumbing::QueryCtxt>
28:     0x7f7711c73f60 - <rustc_query_impl[7c11a1a95cade8b5]::Queries as rustc_middle[f9f9a681d39429ab]::ty::query::QueryEngine>::layout_of
29:     0x7f771205228b - <alloc[2b3e5422ad2b1e63]::vec::Vec<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>> as alloc[2b3e5422ad2b1e63]::vec::spec_from_iter::SpecFromIter<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>, core[2748ca54e7081f8b]::iter::adapters::GenericShunt<core[2748ca54e7081f8b]::iter::adapters::map::Map<core[2748ca54e7081f8b]::slice::iter::Iter<rustc_middle[f9f9a681d39429ab]::ty::FieldDef>, <rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutCx<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt>>::layout_of_uncached::{closure#5}::{closure#0}>, core[2748ca54e7081f8b]::result::Result<core[2748ca54e7081f8b]::convert::Infallible, rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutError>>>>::from_iter
30:     0x7f771205171c - <alloc[2b3e5422ad2b1e63]::vec::Vec<alloc[2b3e5422ad2b1e63]::vec::Vec<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>>> as alloc[2b3e5422ad2b1e63]::vec::spec_from_iter::SpecFromIter<alloc[2b3e5422ad2b1e63]::vec::Vec<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>>, core[2748ca54e7081f8b]::iter::adapters::GenericShunt<core[2748ca54e7081f8b]::iter::adapters::map::Map<core[2748ca54e7081f8b]::slice::iter::Iter<rustc_middle[f9f9a681d39429ab]::ty::VariantDef>, <rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutCx<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt>>::layout_of_uncached::{closure#5}>, core[2748ca54e7081f8b]::result::Result<core[2748ca54e7081f8b]::convert::Infallible, rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutError>>>>::from_iter
31:     0x7f771202f980 - <rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutCx<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt>>::layout_of_uncached
32:     0x7f7712046d06 - rustc_middle[f9f9a681d39429ab]::ty::layout::layout_of
33:     0x7f7711c25196 - <rustc_query_system[ccda753483a689aa]::dep_graph::graph::DepGraph<rustc_middle[f9f9a681d39429ab]::dep_graph::dep_node::DepKind>>::with_task::<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt, rustc_middle[f9f9a681d39429ab]::ty::ParamEnvAnd<rustc_middle[f9f9a681d39429ab]::ty::Ty>, core[2748ca54e7081f8b]::result::Result<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>, rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutError>>
34:     0x7f7711bd67f5 - rustc_query_system[ccda753483a689aa]::query::plumbing::get_query::<rustc_query_impl[7c11a1a95cade8b5]::queries::layout_of, rustc_query_impl[7c11a1a95cade8b5]::plumbing::QueryCtxt>
35:     0x7f7711c73f60 - <rustc_query_impl[7c11a1a95cade8b5]::Queries as rustc_middle[f9f9a681d39429ab]::ty::query::QueryEngine>::layout_of
36:     0x7f771205228b - <alloc[2b3e5422ad2b1e63]::vec::Vec<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>> as alloc[2b3e5422ad2b1e63]::vec::spec_from_iter::SpecFromIter<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>, core[2748ca54e7081f8b]::iter::adapters::GenericShunt<core[2748ca54e7081f8b]::iter::adapters::map::Map<core[2748ca54e7081f8b]::slice::iter::Iter<rustc_middle[f9f9a681d39429ab]::ty::FieldDef>, <rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutCx<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt>>::layout_of_uncached::{closure#5}::{closure#0}>, core[2748ca54e7081f8b]::result::Result<core[2748ca54e7081f8b]::convert::Infallible, rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutError>>>>::from_iter
37:     0x7f771205171c - <alloc[2b3e5422ad2b1e63]::vec::Vec<alloc[2b3e5422ad2b1e63]::vec::Vec<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>>> as alloc[2b3e5422ad2b1e63]::vec::spec_from_iter::SpecFromIter<alloc[2b3e5422ad2b1e63]::vec::Vec<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>>, core[2748ca54e7081f8b]::iter::adapters::GenericShunt<core[2748ca54e7081f8b]::iter::adapters::map::Map<core[2748ca54e7081f8b]::slice::iter::Iter<rustc_middle[f9f9a681d39429ab]::ty::VariantDef>, <rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutCx<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt>>::layout_of_uncached::{closure#5}>, core[2748ca54e7081f8b]::result::Result<core[2748ca54e7081f8b]::convert::Infallible, rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutError>>>>::from_iter
38:     0x7f771202f980 - <rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutCx<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt>>::layout_of_uncached
39:     0x7f7712046d06 - rustc_middle[f9f9a681d39429ab]::ty::layout::layout_of
40:     0x7f7711c25196 - <rustc_query_system[ccda753483a689aa]::dep_graph::graph::DepGraph<rustc_middle[f9f9a681d39429ab]::dep_graph::dep_node::DepKind>>::with_task::<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt, rustc_middle[f9f9a681d39429ab]::ty::ParamEnvAnd<rustc_middle[f9f9a681d39429ab]::ty::Ty>, core[2748ca54e7081f8b]::result::Result<rustc_target[5665e444836c2cfe]::abi::TyAndLayout<rustc_middle[f9f9a681d39429ab]::ty::Ty>, rustc_middle[f9f9a681d39429ab]::ty::layout::LayoutError>>
41:     0x7f7711bd67f5 - rustc_query_system[ccda753483a689aa]::query::plumbing::get_query::<rustc_query_impl[7c11a1a95cade8b5]::queries::layout_of, rustc_query_impl[7c11a1a95cade8b5]::plumbing::QueryCtxt>
42:     0x7f7711c73f60 - <rustc_query_impl[7c11a1a95cade8b5]::Queries as rustc_middle[f9f9a681d39429ab]::ty::query::QueryEngine>::layout_of
43:     0x7f7711c7a555 - rustc_codegen_ssa[8aeb056d12bc5476]::debuginfo::type_names::push_debuginfo_type_name
44:     0x7f7711c79cd6 - rustc_codegen_ssa[8aeb056d12bc5476]::debuginfo::type_names::push_debuginfo_type_name
45:     0x7f7711c7cbc3 - rustc_codegen_ssa[8aeb056d12bc5476]::debuginfo::type_names::push_generic_params_internal
46:     0x7f7711c78a83 - rustc_codegen_ssa[8aeb056d12bc5476]::debuginfo::type_names::push_debuginfo_type_name
47:     0x7f7711c784e0 - rustc_codegen_ssa[8aeb056d12bc5476]::debuginfo::type_names::compute_debuginfo_type_name
48:     0x7f77115250f6 - rustc_codegen_llvm[718acca89ba432ab]::debuginfo::metadata::type_di_node
49:     0x7f7711506df8 - <rustc_codegen_llvm[718acca89ba432ab]::context::CodegenCx as rustc_codegen_ssa[8aeb056d12bc5476]::traits::debuginfo::DebugInfoMethods>::dbg_scope_fn
50:     0x7f77115329b2 - rustc_codegen_ssa[8aeb056d12bc5476]::mir::codegen_mir::<rustc_codegen_llvm[718acca89ba432ab]::builder::Builder>
51:     0x7f77114f1324 - rustc_codegen_llvm[718acca89ba432ab]::base::compile_codegen_unit::module_codegen
52:     0x7f77121afefc - <rustc_query_system[ccda753483a689aa]::dep_graph::graph::DepGraph<rustc_middle[f9f9a681d39429ab]::dep_graph::dep_node::DepKind>>::with_task::<rustc_middle[f9f9a681d39429ab]::ty::context::TyCtxt, rustc_span[71d6b32be7db1b29]::symbol::Symbol, rustc_codegen_ssa[8aeb056d12bc5476]::ModuleCodegen<rustc_codegen_llvm[718acca89ba432ab]::ModuleLlvm>>
53:     0x7f771219e889 - rustc_codegen_llvm[718acca89ba432ab]::base::compile_codegen_unit
54:     0x7f771218cacc - <rustc_codegen_llvm[718acca89ba432ab]::LlvmCodegenBackend as rustc_codegen_ssa[8aeb056d12bc5476]::traits::backend::CodegenBackend>::codegen_crate
55:     0x7f771216de07 - <rustc_session[b6943ffe4755727e]::session::Session>::time::<alloc[2b3e5422ad2b1e63]::boxed::Box<dyn core[2748ca54e7081f8b]::any::Any>, rustc_interface[ed5ba2b564642d99]::passes::start_codegen::{closure#0}>
56:     0x7f771215c878 - <rustc_interface[ed5ba2b564642d99]::passes::QueryContext>::enter::<<rustc_interface[ed5ba2b564642d99]::queries::Queries>::ongoing_codegen::{closure#0}::{closure#0}, core[2748ca54e7081f8b]::result::Result<alloc[2b3e5422ad2b1e63]::boxed::Box<dyn core[2748ca54e7081f8b]::any::Any>, rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>>
57:     0x7f771215524f - <rustc_interface[ed5ba2b564642d99]::queries::Queries>::ongoing_codegen
58:     0x7f7712118d6b - <rustc_interface[ed5ba2b564642d99]::interface::Compiler>::enter::<rustc_driver[689e96f3a05e42de]::run_compiler::{closure#1}::{closure#2}, core[2748ca54e7081f8b]::result::Result<core[2748ca54e7081f8b]::option::Option<rustc_interface[ed5ba2b564642d99]::queries::Linker>, rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>>
59:     0x7f771212bddf - rustc_span[71d6b32be7db1b29]::with_source_map::<core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>, rustc_interface[ed5ba2b564642d99]::interface::create_compiler_and_run<core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>, rustc_driver[689e96f3a05e42de]::run_compiler::{closure#1}>::{closure#1}>
60:     0x7f771212b5e4 - rustc_interface[ed5ba2b564642d99]::interface::create_compiler_and_run::<core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>, rustc_driver[689e96f3a05e42de]::run_compiler::{closure#1}>
61:     0x7f7712116762 - <scoped_tls[a18bab56a8f82071]::ScopedKey<rustc_span[71d6b32be7db1b29]::SessionGlobals>>::set::<rustc_interface[ed5ba2b564642d99]::interface::run_compiler<core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>, rustc_driver[689e96f3a05e42de]::run_compiler::{closure#1}>::{closure#0}, core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>>
62:     0x7f7712114c1f - std[14bdc88f27c1cbe3]::sys_common::backtrace::__rust_begin_short_backtrace::<rustc_interface[ed5ba2b564642d99]::util::run_in_thread_pool_with_globals<rustc_interface[ed5ba2b564642d99]::interface::run_compiler<core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>, rustc_driver[689e96f3a05e42de]::run_compiler::{closure#1}>::{closure#0}, core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>>::{closure#0}, core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>>
63:     0x7f771212cd62 - <<std[14bdc88f27c1cbe3]::thread::Builder>::spawn_unchecked_<rustc_interface[ed5ba2b564642d99]::util::run_in_thread_pool_with_globals<rustc_interface[ed5ba2b564642d99]::interface::run_compiler<core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>, rustc_driver[689e96f3a05e42de]::run_compiler::{closure#1}>::{closure#0}, core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>>::{closure#0}, core[2748ca54e7081f8b]::result::Result<(), rustc_errors[31a36e023e39c2ba]::ErrorGuaranteed>>::{closure#1} as core[2748ca54e7081f8b]::ops::function::FnOnce<()>>::call_once::{shim:vtable#0}
64:     0x7f770faab313 - <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once::hd90ad1cbe107fac7
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/alloc/src/boxed.rs:1853:9
65:     0x7f770faab313 - <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once::h47f9929b43ba9f68
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/alloc/src/boxed.rs:1853:9
66:     0x7f770faab313 - std::sys::unix::thread::Thread::new::thread_start::hf5007aed9e46726b
at /rustc/52b34550aca5f7dd7e152f773e3ab786acb86f6f/library/std/src/sys/unix/thread.rs:108:17
67:     0x7f770f8835c2 - start_thread
68:     0x7f770f908584 - __clone
69:                0x0 - <unknown>

error: internal compiler error: unexpected panic

note: the compiler unexpectedly panicked. this is a bug.

note: we would appreciate a bug report: https://github.com/rust-lang/rust/issues/new?labels=C-bug%2C+I-ICE%2C+T-compiler&template=ice.md

note: rustc 1.61.0-nightly (52b34550a 2022-03-15) running on x86_64-unknown-linux-gnu

note: compiler flags: --crate-type lib -C embed-bitcode=no -C debuginfo=2 -C incremental

note: some of the compiler flags provided by cargo are hidden

query stack during panic:
#0 [is_sized_raw] computing whether `dyn alloc::string::ToString` is `Sized`
#1 [layout_of] computing layout of `*const dyn alloc::string::ToString`
#2 [layout_of] computing layout of `core::ptr::unique::Unique<dyn alloc::string::ToString>`
#3 [layout_of] computing layout of `alloc::boxed::Box<dyn alloc::string::ToString>`
end of query stack
error: could not compile `broken`
```
