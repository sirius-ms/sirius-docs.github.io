---
permalink: /bugs/
title: "Bug reports and feature requests"
---

We strive to ensure that your experience with SIRIUS is error-free, but it's impossible to test every possible scenario. If you encounter an error or have suggestions for new features, we encourage you to submit a bug report / feature request via our GitHub repository ([sirius-ms/sirius](https://github.com/sirius-ms/sirius/issues/new/choose)).

You can also use the bug report feature within the SIRIUS GUI, which allows you to automatically include log files.
Providing your email address is very helpful, as many errors are specific and can only be diagnosed with the input file and the parameters you used. 
We may need to reach out to you for more details to better understand and resolve the issue. Your assistance is invaluable in helping us improve SIRIUS.

Additionally, you can visit our [SIRIUS Community space on Gitter](https://matrix.to/#/#sirius-ms:gitter.im) to connect with other users and seek help from the community.

## Submission templates {#templates}

When submitting a bug report or feature request, please provide us with the information listed below.

### Bug report {#bug-report}

* **SIRIUS Version:** [Current version of SIRIUS being used]
* **Operating System:** [Operating system and version]
* **Hardware:** [Hardware specifications if relevant]

**Bug Description:**
Provide a detailed description of the bug. Include information on what is happening, when it occurs, and any error messages or unexpected behavior. Please also describe what you expected to happen when performing the actions that led to the bug.

**Steps to Reproduce:**
List the steps needed to reproduce the bug. Be as detailed as possible (e.g. include all parameters) to help the development team replicate the issue.

**Screenshots and Log Files:**
Please upload screenshots of any errors and include log files.

**Example data:**
Include any additional information that might help the development team diagnose and fix the bug, in particular relevant project files or datasets, that demonstrate the problem you are reporting.

**Frequency of Occurrence:**
Indicate how often the bug occurs. Does it happen every time, occasionally, or randomly?

**Workarounds:**
If you have found any temporary solutions or workarounds to the bug, please describe them here.

### Feature request {#festure-request}

**Feature Description:**
Provide a detailed description of the feature you are requesting. Be as specific as possible about what the feature should do and how it will improve the current functionality of SIRIUS.

**Use Case / Problem Description:**
Is your feature request related to a problem or a specific use case? Explain how this feature will be used in your workflow or research. Provide examples to illustrate the importance and utility of the feature.

**Benefits:**
List the benefits that the requested feature will bring to the users. Consider aspects such as time savings, improved accuracy, and enhanced functionality.

**Related Issues:**
Does this new feature address or improve any existing issues?

**Additional Information:**
Include any additional information that might be relevant to the feature request. This can include references to scientific literature, links to similar features in other software, screenshots or mockups that could help illustrate the feature, or feedback from other users.

## Common feature requests {#feature-requests}

#### Could SIRIUS support multiply charged ions? {#multiply-charged-ions}

Not in the near future. Implementing support for multiply charged ions requires significant algorithmic development, and currently, we lack sufficient training data for these ions. However, if you can provide such data, it would greatly help initiate this project.

#### Could SIRIUS support multimers? {#multimers}


Supporting multimers is theoretically possible. While they are currently considered 
during LC-MS preprocessing for adduct detection and will appear in the [feature list 
in the GUI]({{ "/gui/#overview" | relative_url}}), 
full computational support is not yet available. This limitation arises because the 
necessary adjustments to the fragmentation tree computation have not been implemented.