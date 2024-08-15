---
permalink: /bugs/
title: "Bug reports and feature requests"
---

We strive to ensure that your experience with SIRIUS is error-free, but it's impossible to test every possible scenario. If you encounter an error or have suggestions for new features, we encourage you to submit a bug report / feature request via our GitHub repository ([sirius-ms/sirius](https://github.com/sirius-ms/sirius/issues/new/choose)).

You can also use the bug report feature within the SIRIUS GUI, which allows you to automatically include log files.
Providing your email address is very helpful, as many errors are specific and can only be diagnosed with the input file and the parameters you used. 
We may need to reach out to you for more details to better understand and resolve the issue. Your assistance is invaluable in helping us improve SIRIUS.

Additionally, you can visit our [SIRIUS Community space on Gitter](https://matrix.to/#/#sirius-ms:gitter.im) to connect with other users and seek help from the community.


## Common feature requests

#### Could SIRIUS support multiply charged ions? 

Not in the near future. Implementing support for multiply charged ions requires significant algorithmic development, and currently, we lack sufficient training data for these ions. However, if you can provide such data, it would greatly help initiate this project.

#### Could SIRIUS support multimers?


Supporting multimers is theoretically possible. While they are currently considered 
during LC-MS preprocessing for adduct detection and will appear in the [feature list 
in the GUI]({{ "/gui/#overview" | relative_url}}), 
full computational support is not yet available. This limitation arises because the 
necessary adjustments to the fragmentation tree computation have not been implemented.