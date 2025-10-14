# Astro Development Guide

## 🎯 AI Agent Instructions for Astro Development

When working with Astro projects, follow these guidelines to ensure optimal AI assistance and code quality.

## 📁 Project Structure

```
src/
├── components/           # Astro components
│   ├── ui/              # Reusable UI components
│   ├── layout/          # Layout components
│   └── features/        # Feature-specific components
├── layouts/             # Page layouts
├── pages/               # File-based routing
│   ├── api/             # API routes
│   └── blog/            # Blog pages
├── content/             # Content collections
│   └── blog/            # Blog posts
├── styles/              # Global styles
├── utils/               # Utility functions
└── types/               # TypeScript definitions
```

## 🚀 Component Development

### Astro Component Template
```astro
---
// Component script (runs at build time)
export interface Props {
  title: string;
  description?: string;
  image?: string;
  date?: Date;
}

const { title, description, image, date } = Astro.props;

// Process data at build time
const formattedDate = date ? new Intl.DateTimeFormat('en-US', {
  year: 'numeric',
  month: 'long',
  day: 'numeric'
}).format(date) : null;
---

<!-- Component markup -->
<article class="card">
  {image && (
    <img 
      src={image} 
      alt={title}
      class="card__image"
      loading="lazy"
    />
  )}
  
  <div class="card__content">
    <h2 class="card__title">{title}</h2>
    
    {description && (
      <p class="card__description">{description}</p>
    )}
    
    {formattedDate && (
      <time class="card__date" datetime={date?.toISOString()}>
        {formattedDate}
      </time>
    )}
  </div>
</article>

<style>
  .card {
    border: 1px solid var(--color-border);
    border-radius: 0.5rem;
    overflow: hidden;
    background: var(--color-bg);
    transition: transform 0.2s ease;
  }
  
  .card:hover {
    transform: translateY(-2px);
  }
  
  .card__image {
    width: 100%;
    height: 200px;
    object-fit: cover;
  }
  
  .card__content {
    padding: 1.5rem;
  }
  
  .card__title {
    font-size: 1.25rem;
    font-weight: 600;
    margin: 0 0 0.5rem 0;
    color: var(--color-text);
  }
  
  .card__description {
    color: var(--color-text-secondary);
    margin: 0 0 1rem 0;
    line-height: 1.6;
  }
  
  .card__date {
    font-size: 0.875rem;
    color: var(--color-text-muted);
  }
</style>
```

### Framework Component Integration
```astro
---
// Using React component in Astro
import ReactComponent from '../components/ReactComponent.tsx';
import VueComponent from '../components/VueComponent.vue';
import SvelteComponent from '../components/SvelteComponent.svelte';
---

<div class="page">
  <h1>Mixed Framework Components</h1>
  
  <!-- React component with client-side hydration -->
  <ReactComponent 
    client:load 
    title="React Component"
    data={someData}
  />
  
  <!-- Vue component with visible hydration -->
  <VueComponent 
    client:visible 
    message="Vue Component"
  />
  
  <!-- Svelte component with idle hydration -->
  <SvelteComponent 
    client:idle 
    count={42}
  />
</div>
```

## 📄 Page Development

### Static Page Template
```astro
---
// pages/about.astro
import Layout from '../layouts/Layout.astro';
import Hero from '../components/Hero.astro';
import Team from '../components/Team.astro';

// Fetch data at build time
const teamMembers = await fetch('https://api.example.com/team').then(res => res.json());
---

<Layout title="About Us" description="Learn more about our team and mission">
  <Hero 
    title="About Our Company"
    subtitle="Building the future of web development"
  />
  
  <main>
    <section class="mission">
      <h2>Our Mission</h2>
      <p>We're dedicated to creating amazing web experiences...</p>
    </section>
    
    <Team members={teamMembers} />
  </main>
</Layout>

<style>
  .mission {
    padding: 2rem 0;
    text-align: center;
  }
  
  .mission h2 {
    font-size: 2rem;
    margin-bottom: 1rem;
  }
</style>
```

### Dynamic Page Template
```astro
---
// pages/blog/[slug].astro
import { getCollection } from 'astro:content';
import Layout from '../../layouts/BlogLayout.astro';

export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map((post) => ({
    params: { slug: post.slug },
    props: { post },
  }));
}

const { post } = Astro.props;
const { Content } = await post.render();
---

<Layout title={post.data.title} description={post.data.description}>
  <article class="post">
    <header class="post__header">
      <h1 class="post__title">{post.data.title}</h1>
      <div class="post__meta">
        <time datetime={post.data.pubDate.toISOString()}>
          {post.data.pubDate.toLocaleDateString()}
        </time>
        <span class="post__author">{post.data.author}</span>
      </div>
    </header>
    
    <div class="post__content">
      <Content />
    </div>
  </article>
</Layout>

<style>
  .post {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem;
  }
  
  .post__header {
    margin-bottom: 2rem;
    text-align: center;
  }
  
  .post__title {
    font-size: 2.5rem;
    margin-bottom: 1rem;
  }
  
  .post__meta {
    color: var(--color-text-secondary);
    font-size: 0.875rem;
  }
  
  .post__content {
    line-height: 1.7;
  }
</style>
```

## 🗂️ Content Collections

### Collection Schema
```typescript
// src/content/config.ts
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
    heroImage: z.string().optional(),
    tags: z.array(z.string()).default([]),
    author: z.string(),
    featured: z.boolean().default(false),
  }),
});

const projects = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    image: z.string(),
    technologies: z.array(z.string()),
    github: z.string().optional(),
    live: z.string().optional(),
    featured: z.boolean().default(false),
  }),
});

export const collections = {
  blog,
  projects,
};
```

### Using Collections
```astro
---
// pages/blog/index.astro
import { getCollection } from 'astro:content';
import Layout from '../../layouts/Layout.astro';
import BlogCard from '../../components/BlogCard.astro';

const posts = await getCollection('blog');
const sortedPosts = posts.sort((a, b) => 
  b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
);
---

<Layout title="Blog" description="Latest posts and articles">
  <main>
    <h1>Blog Posts</h1>
    
    <div class="posts-grid">
      {sortedPosts.map((post) => (
        <BlogCard post={post} />
      ))}
    </div>
  </main>
</Layout>

<style>
  .posts-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 2rem;
  }
</style>
```

## 🎨 Layout System

### Base Layout
```astro
---
// src/layouts/Layout.astro
export interface Props {
  title: string;
  description?: string;
  image?: string;
  canonical?: string;
}

const { title, description, image, canonical } = Astro.props;
const siteTitle = Astro.site?.toString() || 'My Site';
const fullTitle = title === siteTitle ? title : `${title} | ${siteTitle}`;
---

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="description" content={description} />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="generator" content={Astro.generator} />
    
    <!-- Open Graph -->
    <meta property="og:type" content="website" />
    <meta property="og:title" content={fullTitle} />
    <meta property="og:description" content={description} />
    {image && <meta property="og:image" content={image} />}
    
    <!-- Twitter Card -->
    <meta name="twitter:card" content="summary_large_image" />
    <meta name="twitter:title" content={fullTitle} />
    <meta name="twitter:description" content={description} />
    {image && <meta name="twitter:image" content={image} />}
    
    {canonical && <link rel="canonical" href={canonical} />}
    
    <title>{fullTitle}</title>
  </head>
  <body>
    <header>
      <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
        <a href="/blog">Blog</a>
      </nav>
    </header>
    
    <main>
      <slot />
    </main>
    
    <footer>
      <p>&copy; 2024 My Site. All rights reserved.</p>
    </footer>
  </body>
</html>

<style is:global>
  html {
    font-family: system-ui, sans-serif;
  }
  
  body {
    margin: 0;
    line-height: 1.6;
  }
  
  header {
    border-bottom: 1px solid #e2e8f0;
    padding: 1rem 0;
  }
  
  nav {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;
    display: flex;
    gap: 2rem;
  }
  
  nav a {
    text-decoration: none;
    color: #374151;
    font-weight: 500;
  }
  
  nav a:hover {
    color: #1f2937;
  }
  
  main {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem 1rem;
  }
  
  footer {
    border-top: 1px solid #e2e8f0;
    padding: 2rem 0;
    text-align: center;
    color: #6b7280;
  }
</style>
```

## 🔌 API Routes

### API Route Template
```typescript
// src/pages/api/users.ts
import type { APIRoute } from 'astro';

export const GET: APIRoute = async ({ request, url }) => {
  const searchParams = url.searchParams;
  const page = parseInt(searchParams.get('page') || '1');
  const limit = parseInt(searchParams.get('limit') || '10');
  
  try {
    // Fetch data from database or external API
    const users = await fetchUsers({ page, limit });
    
    return new Response(JSON.stringify({
      data: users,
      pagination: {
        page,
        limit,
        total: users.length
      }
    }), {
      status: 200,
      headers: {
        'Content-Type': 'application/json',
      },
    });
  } catch (error) {
    return new Response(JSON.stringify({
      error: 'Failed to fetch users'
    }), {
      status: 500,
      headers: {
        'Content-Type': 'application/json',
      },
    });
  }
};

export const POST: APIRoute = async ({ request }) => {
  try {
    const body = await request.json();
    
    // Validate input
    if (!body.name || !body.email) {
      return new Response(JSON.stringify({
        error: 'Name and email are required'
      }), {
        status: 400,
        headers: {
          'Content-Type': 'application/json',
        },
      });
    }
    
    // Create user
    const user = await createUser(body);
    
    return new Response(JSON.stringify({
      data: user
    }), {
      status: 201,
      headers: {
        'Content-Type': 'application/json',
      },
    });
  } catch (error) {
    return new Response(JSON.stringify({
      error: 'Failed to create user'
    }), {
      status: 500,
      headers: {
        'Content-Type': 'application/json',
      },
    });
  }
};
```

## ⚡ Performance Optimization

### Image Optimization
```astro
---
import { Image } from 'astro:assets';
import heroImage from '../assets/hero.jpg';
---

<!-- Optimized image with multiple formats -->
<Image
  src={heroImage}
  alt="Hero image"
  width={800}
  height={400}
  formats={['avif', 'webp', 'jpg']}
  quality={80}
  loading="eager"
  class="hero-image"
/>
```

### Partial Hydration
```astro
---
// Only hydrate when needed
import InteractiveComponent from '../components/InteractiveComponent.tsx';
---

<div class="page">
  <h1>Static Content</h1>
  <p>This content is rendered at build time.</p>
  
  <!-- Only hydrate on user interaction -->
  <InteractiveComponent client:visible />
  
  <!-- Only hydrate when idle -->
  <AnotherComponent client:idle />
  
  <!-- Only hydrate on media query -->
  <MobileComponent client:media="(max-width: 768px)" />
</div>
```

## 🧪 Testing Patterns

### Component Testing
```typescript
// src/components/__tests__/Card.test.ts
import { expect, test } from 'vitest';
import { render } from '@astrojs/testing';
import Card from '../Card.astro';

test('renders card with title', async () => {
  const { container } = await render(Card, {
    props: {
      title: 'Test Title',
      description: 'Test Description'
    }
  });
  
  expect(container.querySelector('h2')).toHaveTextContent('Test Title');
  expect(container.querySelector('p')).toHaveTextContent('Test Description');
});
```

### Integration Testing
```typescript
// tests/pages.test.ts
import { expect, test } from 'vitest';
import { loadFixture } from '@astrojs/testing';
import { setup, teardown } from './test-utils';

test('homepage loads correctly', async () => {
  const fixture = await loadFixture('./');
  const app = await fixture.load();
  
  const response = await app.request('/');
  expect(response.status).toBe(200);
  
  const html = await response.text();
  expect(html).toContain('<title>Home</title>');
});
```

## 🔧 Common Patterns

### SEO Component
```astro
---
// src/components/SEO.astro
export interface Props {
  title: string;
  description?: string;
  image?: string;
  canonical?: string;
  noindex?: boolean;
}

const { title, description, image, canonical, noindex } = Astro.props;
---

{noindex && <meta name="robots" content="noindex, nofollow" />}
{canonical && <link rel="canonical" href={canonical} />}

<!-- Structured Data -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebPage",
  "name": {title},
  "description": {description}
}
</script>
```

### RSS Feed
```typescript
// src/pages/rss.xml.ts
import type { APIRoute } from 'astro';
import { getCollection } from 'astro:content';

export const GET: APIRoute = async ({ site }) => {
  const posts = await getCollection('blog');
  const sortedPosts = posts.sort((a, b) => 
    b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
  );

  const rss = `<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>My Blog</title>
    <description>Latest blog posts</description>
    <link>${site}</link>
    <atom:link href="${site}/rss.xml" rel="self" type="application/rss+xml"/>
    ${sortedPosts.map(post => `
    <item>
      <title>${post.data.title}</title>
      <description>${post.data.description}</description>
      <link>${site}/blog/${post.slug}/</link>
      <pubDate>${post.data.pubDate.toUTCString()}</pubDate>
    </item>
    `).join('')}
  </channel>
</rss>`;

  return new Response(rss, {
    headers: {
      'Content-Type': 'application/xml',
    },
  });
};
```

## 🚨 Common Pitfalls

1. **Client-side code in server components**: Don't use browser APIs in Astro components
2. **Over-hydration**: Only hydrate components that need interactivity
3. **Large bundle sizes**: Use dynamic imports for heavy components
4. **SEO issues**: Ensure proper meta tags and structured data
5. **Performance**: Optimize images and use proper loading strategies

## 📚 Recommended Integrations

- **Styling**: TailwindCSS, UnoCSS, Styled Components
- **UI Components**: Astro UI, Headless UI
- **Content**: Content Collections, MDX
- **Database**: Drizzle, Prisma, Supabase
- **Deployment**: Vercel, Netlify, Cloudflare Pages
- **Analytics**: Google Analytics, Plausible

## 🎯 AI Agent Best Practices

When assisting with Astro development:

1. **Leverage static generation** for better performance
2. **Use content collections** for structured content management
3. **Optimize for Core Web Vitals** with proper image handling
4. **Implement proper SEO** with meta tags and structured data
5. **Use partial hydration** to minimize JavaScript
6. **Follow Astro's component patterns** for consistency
7. **Test both static and dynamic routes** thoroughly
8. **Use TypeScript** for better type safety
9. **Implement proper error handling** in API routes
10. **Follow accessibility guidelines** with semantic HTML

## 🔗 Additional Resources

- [Astro Documentation](https://docs.astro.build/)
- [Astro Integrations](https://docs.astro.build/en/guides/integrations-guide/)
- [Content Collections](https://docs.astro.build/en/guides/content-collections/)
- [Astro Examples](https://github.com/withastro/astro/tree/main/examples)
- [Astro Discord](https://astro.build/chat)
