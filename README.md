# dorian-naaji.fr

Site vitrine personnel Hugo static site déployé sur Cloudflare Workers.

## Stack

- [Hugo](https://gohugo.io/) - générateur de site statique
- [Cloudflare Workers](https://workers.cloudflare.com/) - hébergement
- CSS custom avec design system (tokens.css)

## Développement

```bash
hugo server
```

## Déploiement

```bash
hugo --minify
npx wrangler deploy
```
