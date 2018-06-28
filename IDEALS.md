Kernel CI Ideals
================
Kernel CI ideals are guiding principles in all our work.

No strings attached
-------------------
* The developers, no matter where they work, should not need any of
  our resources to reproduce the issues we found. E.g.
    * no access required to any hosts we maintain,
    * no need to write us and ask how anything was done,
    * no tests not available publicly are executed.
* All the information required to reproduce the issue should be contained
  inside the CI report message, no extra downloading should be necessary. E.g.
    * which source we used,
    * which patches applied,
    * how we built it,
    * what hardware we used for testing,
    * how we ran the tests,
    * any failure output.
* Extra information which can simplify reproducing the issue may reside on
  publicly-accessible servers and be linked from the message. E.g.
    * kernel images we built,
    * debug info to go with those,
    * exact source tarballs we used.
* The developers should not be required to use any tool to reproduce the issue,
  beside ones they normally use for development, and the tests themselves. I.e.
    * no special download tools,
    * no special build tools,
    * no special test execution tools should be required.
* We may offer tools which help reproduce the problem, but they should not be
  required.

Basically if we all suddenly disappear in a black hole, report recipient
should still be able to reproduce and fix the issue

We inform, don't blame
----------------------
* We phrase all reports in a neutral way, name entities according to their
  function, we are not snarky

We offer help, don't command
----------------------------
* We explain what we did and why, what results we got, and how to reproduce
  them, we don't tell recipients what to do with that

No irrelevant information
-------------------------
* We only send reports that are relevant to the developer's submission.
* We don't send reports just because our code or infrastructure failed.

Participation is voluntary
--------------------------
* We offer a clear and simple way of stopping receiving our reports for both
  internal and external developers and maintainers. It's our job to make our
  testing useful, not developer's job to adjust to them.
* We offer a clear and simple way of starting receiving our reports again, as
  well.

Kernel CI is tested
-------------------
* We deploy the full Kernel CI pipeline and run tests on it automatically, for
  each pull request, and on demand. The tests cover all major features:
  failure modes, all kinds of reports, picking up patches from Patchwork,
  filtering patches, detecting series, etc.
