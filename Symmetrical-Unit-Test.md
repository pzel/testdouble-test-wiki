A unit test is said to be "symmetrical" with its [[subject]] when:

* It's placed in a directory that mirrors the subject and with a name that can be predicted by its filename (e.g. `lib/foo.js` is tested by `lib/foo-test.js`)
* It is the only unit test of the public behavior of the subject
* It does not effectively specify the behavior of the subject's dependencies