# HCI-vježbe-2022-2023

## Vježba 7: Next Image and deploy

U zadnjoj vježbi demonstrirat ćemo kako radi i čemu služi Next Image i kako objaviti stranicu jednom kad je gotova.

### Next Image

Izvori:

https://www.datocms.com/blog/nextjs-images

https://medium.com/eincode/how-to-use-next-js-image-component-dfbf3725b12

> ⚠️ Next 13 mijenja API kojim se koristi NextImage, ali principi rada i prednosti ostaju.

`koji smo već viđali do sad donosi prednosti nad običnim` tagom.
Neke od prednosti uključuju:

-   **Brže učitavanje stranice**: slike Next.js učitavat će se samo pri ulasku u okvir za prikaz, a prema zadanim se postavkama učitavaju odgođeno.
-   **Responzivnost**: slikama će se promijeniti veličina u skladu s korištenim uređajem.
-   **Vizualna stabilnost**: automatski se izbjegava problem kumulativne promjene izgleda. [Više ovdje](https://web.dev/cls/)
-   **Poboljšana izvedba**: Next.js slike mogu se mijenjati u veličini i kodirati na zahtjev, čak i kada su pohranjene na udaljenim poslužiteljima ili vanjskom izvoru podataka, kao što je CMS. To vas sprječava da morate stvarati slike različitih veličina tijekom izrade, što ga čini bržim.

### Korištenje

Za početak potreban je import:

```jsx
import Image from 'next/image';
```

Sljedeće što NextImage treba je `src` atribut koji može biti:

1. Statički importana sika
2. Relativna putanja na sliku u `public` folderu (primjeri u dosadašnjim vježbama)
3. URL na sliku na internetu ili CMS-u / CDN-u

Primjeri:

1. Primjer statične slike:

```jsx
import fooImage from '../public/assets/foo.jpg';
```

i onda

```jsx
<>
    {/*// ...*/}
    <Image
        src={fooImage}
        alt="Foo image"
        layout={'fill'}
        // ...
    />
    {/*// ...*/}
</>
```

2. Primjer za relativnu putanju na sliku u `public`:

```jsx
<Image src={'/assets/hero.png'} alt="Foo image" layout={'fill'} />
```

3. Primjer za URL:

```jsx
<Image
    src={'https://assets-global.website-files.com/neka_slika.jpg'}
    alt="Foo image"
    layout={'fill'}
/>
```

Ono što je bitno za javne slike je postavljanje whitelist domena za te slike.

Iz sigurnosnih razloga Next ne dopušta da samo stavimo URL na sliku nego je potrebno i dodati domenu te slike u listu dopuštenih domena (tkz. whitelist)

Ako pogledamo u `next.config.js` možemo vidit definirane domene koje smo koristili na vježbama:

```js
module.exports = {
    reactStrictMode: true,
    images: {
        remotePatterns: [
            {
                protocol: 'https',
                hostname: 'assets-global.website-files.com',
            },
            {
                protocol: 'https',
                hostname: 'i.ytimg.com',
            },
            {
                protocol: 'https',
                hostname: 'play-lh.googleusercontent.com',
            },
            {
                protocol: 'https',
                hostname: 'pbs.twimg.com',
            },
            {
                protocol: 'https',
                hostname: 'scrimba.com',
            },
        ],
    },
};
```

Ako koristimo ``na Nextu 13 ne trebamo`layout` prop.

## SEO i meta tags

SEO je dio dizajna. Stranica treba imati naslov vidljiv u browseru, infomracije koje će prikazati u Google pretragi, ikonu i sl.

SEO je i dosta bitan za poslovne ciljeve. Detaljnije ovdje:
https://www.metricmarketing.com/blog/the-importance-of-seo-for-your-business-benefits-of-seo-why-seo-is-so-powerful/

Sljedi primjer dodavanja SEO-a u Next koristeći third party paket: [Next-SEO](https://www.npmjs.com/package/next-seo?activeTab=readme).

```jsx
<NextSeo
    title="HCI 2022/2023"
    description="A short description goes here."
    openGraph={{
        url: 'http://marjan.fesb.hr/~mcagalj/',
        title: 'Our cool HCI page!',
        description: 'Learn how to make pages using NEXT JS!',
        images: [
            {
                url: 'https://res.cloudinary.com/mcagalj/image/upload/f_auto,c_limit,w_128,q_auto/v1636883352/next_course/logo_t6nqep.png',
            },
        ],
        siteName: 'SiteName',
    }}
    twitter={{
        handle: '@handle',
        site: '@site',
        cardType: 'summary_large_image',
    }}
/>
```

Za testiranje možemo koristiti `localtunnel` i `https://www.opengraph.xyz/`.

## Deploy

Za deploy koristit ćemo **Vercel**! Platformu tvoraca Next-a. Alternativno, možemo koristiti **Netlify**, privatni VPS ili RasberyPI (extra bodovi za ovo 😁).

Deploy ćemo raditi pomoćuu GitHub-a. Svaki novi push u `main` branch aktivirat će ponovni deploy.  
Deploy korak radi build Next aplikacije i objavljuje stranicu javno. Spajanje tog koraka na GitHub primjer je automatizacijedeployaa koja se još zove i **Continuous Deployment**

Započetak registrirajmo se naVercell koristeći Githubb Account:
[Vercel](https:/vercell.com)

Zatim idemo na stvaranje novog projekta i biramo našrepoo.

<p align='center'>
  <img src='public/Deploy/pick_a_repo.png'>
</p>

Zatim na koraku možemo postaviti konfiguraciju. Za sada nam ne treba:

<p align='center'>
  <img src='public/Deploy/finish.png'>
</p>

Ako je sve prošlo ok trebali bismo vidjeti nešto ovako:

<p align='center'>
    <img src='public/Deploy/sucessful_deploy.png' />
</P>
