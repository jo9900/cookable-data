# cookable-data

Recipe data for the **烧了吧 (cookable)** iOS app — see https://github.com/jo9900/Cookable.

This repo contains the master JSON files served via GitHub raw to the iOS app at runtime. Users get menu updates without an app re-release.

## Files

- `methods.json` — cooking methods catalog (炒/煮/烤/...)
- `ingredients.json` — ingredients / condiments / spices catalogs
- `recipes.json` — recipe definitions with required vs optional capability lists
- `version.json` — version probe (the app fetches this small file first to decide what else to refresh)

## URLs

```
https://raw.githubusercontent.com/jo9900/cookable-data/main/version.json
https://raw.githubusercontent.com/jo9900/cookable-data/main/methods.json
https://raw.githubusercontent.com/jo9900/cookable-data/main/ingredients.json
https://raw.githubusercontent.com/jo9900/cookable-data/main/recipes.json
```

## How to update

Edit the source generator in the main app repo:

```
~/Desktop/others/cookable/scripts/generate_data.py
```

Then sync to this repo via:

```
cd ~/Desktop/others/cookable && python3 scripts/generate_data.py
cp data/*.json ~/Desktop/others/cookable-data/
cd ~/Desktop/others/cookable-data && git add -A && git commit -m "data: ..." && git push
```

(Or just use `scripts/push_data.sh` from the main repo — see that script for one-command sync + push.)
