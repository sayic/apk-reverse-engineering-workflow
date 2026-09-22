# Workflow

1. Establish the user's goal, read the shared rules, relevant workflow, APK information, and related task records, and first determine whether existing results can be reused.
2. Start package extraction with extract; start with download when the user explicitly requests remote downloads. Call container as required by the inputs, then art, code, and config according to resource type.
3. Reuse container parsing results based on inputs, scope, tools, and parameters; do not repeat parsing merely because conversion begins. Subflows share the main task record.
4. Put intermediate materials in the appropriate task directory under unpacked/. Put organized results in outputs at the task workspace root, with file indexes and detailed reports alongside them. context records artifact entry points, stages, issues, and continuation; info records only APK information.
5. Use query for subsequent resource lookup, repair for resource fixes, mapping for target-table changes, and import for art migration between projects. Perform these operations as requested by the user.

By default, perform validation relevant to the current task. If the user will validate independently or asks to skip validation, record it as not performed. Record file generation, static checks, Unity import, visual results, and business integration separately. See the [execution rules](../rules/execution.md) and [delivery rules](../rules/delivery.md).

See [tool documentation](../tools/README.md) for required external implementations and interface examples. This skill does not bundle those executables. Complete APK processing and Unity integration require task-specific environments and validation.
