# STAview

A STA (Static Timing Analysis) report file viewer.

This script takes in a STA file and renders it in an interactive graphical format within an ASCII terminal.
The data is rendered as a "slack histogram", where timing paths are binned according to timing margin relative
to the clock constraint. The histogram is zoomable.

Each bin of the histogram is selectable to reveal a list of paths in the bin; each path can be selected
to pull up further detail about the path by pressing "enter" on the selected path (navigation between
windows is accomplished via the "tab" and "shift-tab" keys).

Finally, the paths to be analyzed may be filtered by either typing a string into the "filter" box, or
a Python regex in the format of `r"regex"`, where `regex` is any valid Python regex string. A `-` in front
of the filter expression causes the filter to exclude paths instead of designating which ones to include.

The STA file is specified with the `--report` argument. If `--output` is specified, the STA file is
saved as a redacted JSON which removes library-specific details from the timing report. If the file format
specified to the tool ends in `.json,` it is assumed to be a previously saved JSON and is directly loaded
into the tool. JSON loading is significantly faster than STA parsing, and thus can save time when navigating
extremely large timing reports.

A timing report suitable for analysis can be generated from design compiler using the following command:

`report_timing -delay_type max -max_paths 10000000 -nworst 1 -path_type full_clock -sort_by group > timing.rpt`

Below is a static example of the tool output.

```
max=264 selected=29(0.0400ns)                                                                                                          ──┐
             .                                                                                                                           │
             .                                                                                                                           │
             .                                                                                                                           │
       .   . .                                                                                                                           │
       .   .... .  ..                                                                                                                    │
     . .  ...........                                                                                                                    │
     ................                                                                                                                    │
     ................                                                                                                                    │
    .................      .                                                                                                             │
   .................. ..   .                                                                                                             │
   .................. .. ...                                                                                                             │
   .................. .. ... .                                                                                                           │
   .................. .. ..... .                        .                                                                                │
   ........................... .               ..       .                                                                                │
  ...............................             ...  .  . .. .                                                                             │
 ................................          .  .... .. . .. .                                                                             │
 .................................         .  ............ .                                                                             │
 ..................................  .     .. ............... ..                                                                         │
 .................................. ..     .. ............... ...                                                                        │
 .................................. ..  . .......................  .                                                                     │
 .................................. ............................. ..                                                                     │
 ................................................................ ...                                                                    │
 .....................................................................                                                                   │
 .....................................................................                                                                   │
 .....................................................................                                                                   │
 ..................................................................... .               .                                                 │
 ........................................................................              .                                                 │
 .........................................................................            ..                                                 │
 ......................................................................... .  .     .... .                                               │
 ..............................................................................  .. ........                                             │
 ............................................................................... .. ......... ..                   ..                    │
 ...............................................................................#...............  ... .        . ....                    │
 ...............................................................................#................ .........  ........   .                │
................................................................................#.............................................  .        │
-0.001          0.007           0.016           0.024           0.032           0.040           0.048           0.056           0.064    │
│                                                                                                                                        │
│   u_dmem/gen_par_scr_0__u_prim_prince/data_state_middle_q_reg* -> u_otbn_core/u_otbn_alu_bignum/mod_intg_q_reg* (0.04)                 │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_61__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_61__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_67__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_93__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_52__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_43__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_controller/u_otbn_loop_controller/g_loop_counters_0__u_loop_count/gen_cnts_0__u_cnt_flop/q_o_reg* -> u_otbn_cor   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_controller/u_otbn_loop_controller/g_loop_counters_2__u_loop_count/gen_cnts_0__u_cnt_flop/q_o_reg* -> u_otbn_cor   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_93__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_52__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_insn_fetch_resp_data_intg_flop/q_o_reg* -> u_otbn_core/u_otbn_rf_bignum/gen_rf_bignum_ff_u_   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_93__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_52__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_76__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_dmem/u_prim_ram_1p_adv/rvalid_sram_q_reg* -> u_otbn_core/u_otbn_controller/u_otbn_loop_controller/g_loop_counters_7__u_loop_coun   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_52__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_93__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│   u_otbn_core/u_otbn_instruction_fetch/u_rf_predec_bignum_flop/gen_ffrs_43__gen_negrs_u_ffr -> u_otbn_core/u_otbn_rf_bignum/gen_rf_b   │
│                                                                                                                                        │
│                                                                                                                                        │
│                                                                                                                                        │
│                                                                                                                                        │
│                                                                                                                                        │
│                                                                                                                                        │
│Filter:                                                                                                                                 │
│                                                                                                                                        │
│                                                                                                                                        │
│                                                                                                                                        │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

Below is an animated GIF of the tool in action.

![Animated GIF of staview in action](./staview.gif)
