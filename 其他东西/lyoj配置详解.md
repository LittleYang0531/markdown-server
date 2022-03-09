# lyoj Config File Explanation

## Object "web"

| key name | value type | explanation |
| :------: | :--------: | :---------: |
| name | string | Website name |
| title | string | Website name which will be shown in title |
| absolute_path | bool | Enable absolute path in website link |
| protocol | string | Website protocol (http&https) |
| domain | string | Website address |
| icon | string | Icon file path which will be shown in title |
| logo | string | Icon file path which will be shown in main website |
| default_path | string | Default page when no path input |
| url_rewrite | bool | Enable to rewrite url (`/index.php?path=xxx&param`->`/xxx?param`) |
| menu | object | Menu info in main website |
| footer | object | Footer info in main website |

### Object menu

