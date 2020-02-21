Steps to reproduce multiple issues:

1. Have yarn 1.22.0
2. Have rush 5.20.0
3. Run `rush update`
4. All works as expected (check `node_modules` of all packages)
5. Go to `already-added` module and add `@types/lodash` into `dependencies`
6. `rush update` (check node_modules of `already-added`)
7. Works as expected, `@types/lodash` is added 
8. Remove `@types/lodash` from `package.json`
9. `rush update`
10. It stays in `node_modules`
11. `rush update --full`
12. It's gone. Good.
13. Go add `add-me` to `dependencies` of `add-to-me`
14. Run `rush update`
15. Check `node_modules` of `add-to-me` there is NO `add-me` package added. (same applies to `rush-link.json`)
16. Run `rush update --full`
17. Nothing changed, same issue.
18. Add `@types/lodash` to `add-to-me` into `dependencies`
19. Run `rush update` (check `node_modules` of `add-to-me`)
20. `@types/lodash` is not added to `node_modules` nor to `yarn.lock`
21. Run `rush update --full`
22. Nothing changed. `@types/lodash` is not added to `node_modules` nor to `yarn.lock`
23. Add `@types/node` to `already-added` into `dependencies`
24. Run `rush update` (check node_modules of `already-added`)
25. For some weird reason `@types/lodash` is added instead of `@types/node`
26. Run `rush update --full` (check node_modules of `already-added`)
27. `@types` folder got removed completely.
28. Add `@types/lodash` to `already-added` into `dependencies`
29. Run `rush update --full` (check node_modules of `already-added`)
30. Same, nothing changed. Only `lodash` stays there. Meanwhile `yarn.lock` is untouched since our first update
31. Add `@types/uuid` to `already-added` to `devDependencies` and run `rush update`
32. It detects that something is have to be added! And adds only `@types/lodash` and nothing else.

Any manual deletion  of `yarn.lock` of `rush-link.json` + `rush update`, or whatever commands like `unlink` `link` doesn't fix the issue
Only `rush update --full --purge` fixes it all. But it's too long for real projects to make this all the time.