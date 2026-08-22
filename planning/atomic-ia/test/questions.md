# Test Questions — Atomic IA Draft v1

45 realistic questions a QRtub user (small-to-medium trades/facilities business, not a
developer) might type into search or ask support. Weighted toward probing atomic
granularity — narrow concepts this redesign was supposed to give their own page — with a
handful of broader questions to sanity-check the rest of the tree.

1. What exactly is a "slug" in one of my QR code links?
2. Why do some of my links only work in lowercase, but others let me use capital letters?
3. Is the way new links get created (random vs numbered) something I set once per Collection, or do I choose it every time I add an Item?
4. If I delete a whole Collection, do the physical QR codes already stuck on my equipment stop working?
5. What's the actual difference between deleting a Link and just unassigning it from an Item?
6. What does it mean when the app says a link got "claimed" after someone scanned an unknown code?
7. Is "Conditional Visibility" the same feature as "Device Detection," or are those two different things?
8. What's the difference between Conditional Visibility and Conditional Destinations — aren't they both just "show different stuff based on rules"?
9. What is a "Destination," exactly — is that different from the Link itself, or from the Page?
10. I set up a custom Item ID pattern and it's warning me about a "mask conflict" — what does that actually mean?
11. What's the difference between a "core field" and a "custom field" on my Collection?
12. If I mark a field as Required, does that block a CSV import that's missing a value for it?
13. If I rename a custom field, does it break anything that was already pointing at the old name?
14. Can I permanently delete a field, or can some fields only be turned off?
15. What's a Reference field, and how is that different from just picking a value off a list?
16. What does the "Allow New Values" toggle actually let someone do?
17. What's the "Multiple values" checkbox for, and why don't I see it on every field type?
18. Is Direct Mode vs Page Mode a whole-Collection setting, or can one Item be different from the rest?
19. If I turn on the Per-Item Page Override for one Item, does that change the template for every other Item too?
20. What's the difference between a Page Template Version and a Starter Template?
21. What's an "Unallocated Link," and why would I ever want a QR code that isn't attached to anything?
22. What's the real difference between a Random Link, a Numbered Link, and a Custom Link?
23. Can I unassign a bunch of Links from Items all at once, or do I have to do it one by one?
24. Don't App Links/Fallback URLs and Device Detection both just decide where a scan goes based on the phone? What's actually different about them?
25. In the {{ }} fields, what's the difference between the item, tub, device, time, and request namespaces?
26. Can I write something like {{time.today}} in a Destination to check if an inspection is overdue?
27. If I use {{request.country}} in a link, is that tracking where my customers are scanning from?
28. What's the difference between a Print Batch's overall status and the "deployment status" of one code inside it?
29. If I archive a print batch, is that the same as deleting it?
30. Why can't I edit which columns are in a print batch's CSV once it's left Draft?
31. There's a "claim-on-scan" thing when I look up Links, and a separate one for the in-app scanner — are those the same feature?
32. What's the "Default Destination" setting on a Collection, and how is it different from setting a Destination on one specific Item?
33. If I export a Collection backup, does that include all my Items, or just the settings?
34. What's the difference between "Merge" and "Replace" when importing a Collection backup?
35. Can I use a backup file to spin up a brand-new Collection, or does it only restore into the Collection I exported it from?
36. Is disabling a core field the same thing as deleting it?
37. What actually happens if someone types a Tag that isn't already on the list?
38. When I duplicate an Item, does the copy keep the original's Item ID?
39. What is "Page Metadata" actually controlling — is that just the page title?
40. Are ActionLink and AdminToolbar two names for the same section, or genuinely different?
41. What's the difference between Variable Data Printing and Gang Sheets at a print shop?
42. What's this "quiet zone" my print shop keeps asking me about?
43. If someone's role shows as "Owner" on my team, does that mean they actually have owner powers?
44. Does my subscription attach to my personal login, or to the Team?
45. What counts toward my plan's limits — is it the number of Collections, Links, Items, or team seats?
