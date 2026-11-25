# PR 19719 screenshots: Insert Design's options and linked subassemblies, 1280×720

Fresh pictures from the PR branch `linked-subassemblies-5526` at `306f748a28`, captured on macOS
aarch64 with Metal rendering, in a 1280×720 window (`800×600` for the small-window check).

The designs in the pictures were made for them: `base.solidcraft` (a plate), `arm.solidcraft`
(a leaf), and `hinge.solidcraft`, which links both as components and joins them with a revolute
joint called Hinge. `assembly.solidcraft` is a new design that links the hinge.

| Picture | What it shows |
|---|---|
| `01-ribbon-assemble.png` | The Assemble ribbon tab and its Insert button, with the hinge design open in another tab. |
| `02-insert-design-menu.png` | Insert Design lists the designs open in other tabs, then Browse… |
| `03-insert-design-options.png` | Insert Design's own options panel once hinge is picked: Keep its components off, Flexible off and greyed. |
| `04-insert-design-both-ticked.png` | Keep its components and Flexible both ticked, before OK. |
| `05-browser-linked-subassembly.png` | The Browser: the linked hinge with its base and arm kept inside it, each marked with a lock. |
| `06-occurrence-make-flexible.png` | The occurrence's menu, Component submenu open: Make Flexible (the second occurrence is still rigid here). |
| `07-linked-component-keep-its-components.png` | A plain linked component's menu: Keep Its Components, on a link that did not keep them. |
| `08-result-two-poses.png` | Two flexible occurrences of one hinge, each driven to its own angle (60° and −60°). |
| `09-bill-of-materials.png` | The bill of materials: the kept components counted in every occurrence (2 rows, 4 placed), with their part properties. |
| `10-small-window-menu.png` | Small-window check at 800×600: the Insert Design list. |
| `11-small-window-panel.png` | Small-window check at 800×600: the options panel, OK and Cancel included. |
