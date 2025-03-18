# Fullstack Blog: Next.js + Supabase + Shadcn UI

This file outlines a set of detailed tasks for building a personal fullstack blog using Next.js, Supabase, and Shadcn UI/Tailwind for styling.

Run through each task step by step. After completing each task, mark it as completed with an `[x]` and commit your changes. If you need more detail about a task, gather relevant files and pass the FULL file to the research agent.

Suggested prompt to use with the cursor agent:
``` txt
Go through each task in the init.d file. After you complete each task, update the file to check off any task. Run builds and commits after each task. Continue with each task until you have checked off each one. After each story, do not take a screenshot. If you need more detail about a task, you can gather relevant files and pass the FULL file to the research agent.
```

## 1. Project Setup

1. [ ] **Set up the Supabase client**
   - [ ] Create a `src/lib/supabase.ts` file to initialize the Supabase client:
   - [ ] Verify the environment variables are correctly set in `.env.local`

2. [ ] **Create database types for TypeScript**
   - [ ] Create `src/lib/database.ts` file with the type definitions based on our schema:

3. [ ] **Create utility functions for data fetching**
   - [ ] Create `src/lib/api.ts` with functions to interact with Supabase: getAllPosts, getPostBySlug and getAllCategories

## 2. Set Up Shadcn UI Components

1. [ ] **Install and configure Shadcn UI**
   - [ ] Run the following commands to set up Shadcn UI:
     ```bash
     npm install @shadcn/ui
     npx shadcn-ui init
     ```
   - [ ] When prompted, select the following options:
     - Style: Default
     - Base color: Slate
     - Global CSS: src/app/globals.css
     - CSS variables: Yes
     - React Server Components: Yes
     - Tailwind CSS config: tailwind.config.js
     - Components directory: src/components
     - Utility directory: src/lib/utils

2. [ ] **Install necessary Shadcn UI components**
   - [ ] Install the card component:
     ```bash
     npx shadcn@latest init
     ```
   - [ ] Install the button component:
     ```bash
     npx shadcn@latest add button
     ```
   - [ ] Install other necessary components:
     ```bash
     npx shadcn@latest add avatar
     npx shadcn@latest add badge
     npx shadcn@latest add separator
     ```

3. [ ] **Set up global styles**
   - [ ] Update `src/app/globals.css` to include your custom styling as needed

## 3. Create Blog UI Components

1. [ ] **Create a markdown renderer component**
   - [ ] Install necessary packages:
     ```bash
     npm install react-markdown rehype-raw rehype-sanitize remark-gfm
     ```
   - [ ] Create `src/components/blog/markdown-renderer.tsx` to render markdown content

2. [ ] **Create a blog post card component**
   - [ ] Create `src/components/blog/post-card.tsx` to render a blog post card
3. [ ] **Create blog post grid component**
   - [ ] Create `src/components/blog/post-grid.tsx`, this is an example of a post grid component:
      ```typescript
      import React from 'react';
      import { PostCard } from './post-card';
      import { Post } from '@/lib/api';

      interface PostGridProps {
        posts: Post[];
      }

      export function PostGrid({ posts }: PostGridProps) {
        return (
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {posts.map((post) => (
              <PostCard key={post.id} post={post} />
            ))}
          </div>
        );
      }
      ```

4. [ ] **Create a blog post header component**
   - [ ] Create `src/components/blog/post-header.tsx`, this is an example of a post header component:
     ```typescript
     import React from 'react';
     import Image from 'next/image';
     import { Badge } from '@/components/ui/badge';
     import { Post } from '@/lib/api';

     interface PostHeaderProps {
       post: Post;
     }

     export function PostHeader({ post }: PostHeaderProps) {
       return (
         <div className="space-y-6">
           <div className="space-y-2">
             {post.categories && (
               <Badge variant="outline">{post.categories.name}</Badge>
             )}
             <h1 className="text-3xl md:text-4xl font-bold tracking-tight">
               {post.title}
             </h1>
             <p className="text-muted-foreground">
               {new Date(post.created_at).toLocaleDateString('en-US', {
                 weekday: 'long',
                 month: 'long',
                 day: 'numeric',
                 year: 'numeric',
               })}
             </p>
           </div>
           {post.featured_image && (
             <div className="aspect-video relative rounded-lg overflow-hidden">
               <Image
                 src={post.featured_image}
                 alt={post.title}
                 fill
                 className="object-cover"
                 priority
                 sizes="(max-width: 768px) 100vw, 1200px"
               />
             </div>
           )}
         </div>
       );
     }
     ```

## 4. Implement Blog Pages

1. [ ] **Create the blog listing page**
   - [ ] Create `src/app/blog/page.tsx` to display all blog posts

2. [ ] **Create the blog post detail page**
   - [ ] Create `src/app/blog/[slug]/page.tsx` to display a single blog post

3. [ ] **Create a Not Found page**
   - [ ] Create `src/app/blog/not-found.tsx`

## 5. Update the Homepage

1. [ ] **Update the homepage to showcase recent blog posts**
   - [ ] Update `src/app/page.tsx`:

## 6. Create Layout and Navigation

1. [ ] **Update the root layout**
   - [ ] Update `src/app/layout.tsx`, this is an example of a root layout:
     ```typescript
     import './globals.css';
     import type { Metadata } from 'next';
     import { Inter } from 'next/font/google';
     import Header from '@/components/header';
     import Footer from '@/components/footer';

     const inter = Inter({ subsets: ['latin'] });

     export const metadata: Metadata = {
       title: {
         template: '%s | My Personal Blog',
         default: 'My Personal Blog',
       },
       description: 'A personal blog built with Next.js and Supabase',
     };

     export default function RootLayout({
       children,
     }: {
       children: React.ReactNode;
     }) {
       return (
         <html lang="en">
           <body className={inter.className}>
             <div className="flex flex-col min-h-screen">
               <Header />
               <main className="flex-1">{children}</main>
               <Footer />
             </div>
           </body>
         </html>
       );
     }
     ```

2. [ ] **Create header component**
   - [ ] Create `src/components/header.tsx`, this is an example of a header component:
     ```typescript
     import Link from 'next/link';
     import { Button } from '@/components/ui/button';

     export default function Header() {
       return (
         <header className="border-b">
           <div className="container mx-auto flex items-center justify-between h-16 px-4">
             <Link href="/" className="font-bold text-xl">
               My Blog
             </Link>
             <nav className="flex items-center gap-4">
               <Button variant="ghost" asChild>
                 <Link href="/">Home</Link>
               </Button>
               <Button variant="ghost" asChild>
                 <Link href="/blog">Blog</Link>
               </Button>
             </nav>
           </div>
         </header>
       );
     }
     ```

3. [ ] **Create footer component**
   - [ ] Create `src/components/footer.tsx`, this is an example of a footer component:
     ```typescript
     import { Separator } from '@/components/ui/separator';

     export default function Footer() {
       const currentYear = new Date().getFullYear();

       return (
         <footer className="border-t py-8">
           <div className="container mx-auto px-4">
             <div className="flex flex-col md:flex-row justify-between items-center gap-4">
               <div className="text-center md:text-left">
                 <p className="text-sm text-muted-foreground">
                   © {currentYear} My Personal Blog. All rights reserved.
                 </p>
               </div>
             </div>
           </div>
         </footer>
       );
     }
     ```

## 7. Configure Database and Create Initial Content

1. [ ] **Set up Supabase database**
   - [ ] Create a new Supabase project if you haven't already:
     - Go to https://app.supabase.com and sign in
     - Click "New Project"
     - Enter a project name and database password
     - Choose the region closest to you
     - Click "Create new project"
   - [ ] Run the SQL setup script in the Supabase SQL editor:
     - In your Supabase project dashboard, click on "SQL Editor" in the left sidebar
     - Click the "New Query" button in the top right corner
     - Paste the entire SQL script below into the editor
     - Click the "Run" button (or press Ctrl+Enter / Cmd+Enter) to execute the script
     - Verify that the tables were created by checking the "Table Editor" section
     ```sql
     -- Create tables
     CREATE TABLE categories (
       id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
       name TEXT NOT NULL,
       slug TEXT NOT NULL UNIQUE,
       created_at TIMESTAMP WITH TIME ZONE DEFAULT now() NOT NULL
     );

     CREATE TABLE posts (
       id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
       title TEXT NOT NULL,
       slug TEXT NOT NULL UNIQUE,
       content TEXT,
       featured_image TEXT,
       excerpt TEXT,
       category_id UUID REFERENCES categories(id),
       published BOOLEAN DEFAULT false,
       published_at TIMESTAMP WITH TIME ZONE,
       created_at TIMESTAMP WITH TIME ZONE DEFAULT now() NOT NULL,
       updated_at TIMESTAMP WITH TIME ZONE DEFAULT now() NOT NULL
     );

     -- Set up RLS policies
     ALTER TABLE categories ENABLE ROW LEVEL SECURITY;
     ALTER TABLE posts ENABLE ROW LEVEL SECURITY;

     -- Categories policies
     CREATE POLICY "Categories are viewable by everyone" ON categories
       FOR SELECT USING (true);

     CREATE POLICY "Only authenticated users can create categories" ON categories
       FOR INSERT WITH CHECK (auth.role() = 'authenticated');

     -- Posts policies
     CREATE POLICY "Published posts are viewable by everyone" ON posts
       FOR SELECT USING (published = true);

     -- Set up realtime
     BEGIN;
       DROP PUBLICATION IF EXISTS supabase_realtime;
       CREATE PUBLICATION supabase_realtime FOR TABLE posts, categories;
     COMMIT;
     ```

2. [ ] **Add initial seed data**
   - [ ] Create seed data for categories:
     ```sql
     INSERT INTO categories (name, slug) VALUES
     ('Technology', 'technology'),
     ('Personal', 'personal'),
     ('Projects', 'projects');
     ```
   - [ ] Create seed data for a sample blog post:
     ```sql
        INSERT INTO posts (
        title,
        slug,
        content,
        excerpt,
        category_id,
        published,
        published_at,
        updated_at
        ) VALUES (
        'Getting Started with Next.js and Supabase',
        'getting-started-with-nextjs-and-supabase',
        E'# Getting Started with Next.js and Supabase\n\nNext.js and Supabase make a powerful combination for building modern web applications. In this post, I''ll share my experience setting up a blog with these technologies.\n\n## Why Next.js?\n\nNext.js offers several advantages:\n\n- **Server-Side Rendering (SSR)** - Great for SEO\n- **Static Site Generation (SSG)** - Fast page loads\n- **API Routes** - Backend functionality without a separate server\n- **Built-in Image Optimization** - Faster page loads and better UX\n\n## Why Supabase?\n\nSupabase provides:\n\n- **PostgreSQL Database** - Powerful and reliable\n- **Authentication** - Easy to implement\n- **Storage** - For images and other files\n- **Realtime** - For live updates\n\n## Code Example\n\nHere''s a simple example of fetching data from Supabase in Next.js:\n\n```typescript\nimport { createClient } from ''@supabase/supabase-js''\n\nconst supabase = createClient(\n  process.env.NEXT_PUBLIC_SUPABASE_URL!,\n  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!\n)\n\nasync function getPosts() {\n  const { data, error } = await supabase\n    .from(''posts'')\n    .select(''*'')\n    .order(''created_at'', { ascending: false })\n  \n  if (error) {\n    console.error(''Error fetching posts:'', error)\n    return []\n  }\n  \n  return data\n}\n```\n\n## Conclusion\n\nNext.js and Supabase provide a solid foundation for building modern web applications. They''re especially well-suited for content-heavy sites like blogs.',
        'Learn how to combine Next.js and Supabase to create a powerful and flexible blog application with server-side rendering and a PostgreSQL database.',
        (SELECT id FROM categories WHERE slug = 'technology'),
        true,
        now(),
        now()
        );
     ```
   - [ ] Add a second blog post for testing:
     ```sql
       INSERT INTO posts (
          title,
          slug,
          content,
          excerpt,
          category_id,
          published,
          published_at,
          updated_at
        ) VALUES (
          'Creating a Responsive UI with Tailwind CSS',
          'creating-responsive-ui-with-tailwind-css',
          E'# Creating a Responsive UI with Tailwind CSS\n\nTailwind CSS has revolutionized the way I approach frontend development. In this post, I''ll share some tips on creating responsive designs with Tailwind.\n\n## Why Tailwind CSS?\n\nTailwind offers several advantages:\n\n- **Utility-First** - Build custom designs without leaving your HTML\n- **Responsive Design** - Built-in responsive modifiers\n- **Dark Mode** - Easy dark mode implementation\n- **Customization** - Tailor the framework to your needs\n\n## Responsive Design Example\n\nHere''s a simple responsive card component using Tailwind:\n\n```\n<div class="max-w-sm md:max-w-lg lg:max-w-xl mx-auto bg-white rounded-lg shadow-md overflow-hidden">\n  <div class="md:flex">\n    <div class="md:shrink-0">\n      <img class="h-48 w-full object-cover md:w-48" src="/img/example.jpg" alt="Example Image">\n    </div>\n    <div class="p-8">\n      <h2 class="block mt-1 text-lg leading-tight font-medium text-black">Card Title</h2>\n      <p class="mt-2 text-slate-500">This card changes layout based on screen size.</p>\n    </div>\n  </div>\n</div>\n```\n\n## Using Tailwind with React Components\n\nWhen using Tailwind with React components, you can create reusable UI elements:\n\n```\nfunction Button({ children, primary = false }) {\n  const baseClasses = "px-4 py-2 rounded font-semibold text-sm focus:outline-none focus:ring-2";\n  const colorClasses = primary\n    ? "bg-blue-500 hover:bg-blue-600 text-white"\n    : "bg-gray-200 hover:bg-gray-300 text-gray-800";\n  \n  return (\n    <button className={`${baseClasses} ${colorClasses}`}>\n      {children}\n    </button>\n  );\n}\n```\n\n## Conclusion\n\nTailwind CSS provides a powerful toolkit for building responsive interfaces. Its utility-first approach allows for rapid development while maintaining full control over your design.',
          'Discover how to build responsive user interfaces using Tailwind CSS, a utility-first CSS framework that makes it easy to create custom designs.',
          (SELECT id FROM categories WHERE slug = 'technology'),
          true,
          now(),
          now()
        );
      ```
      
## 8. Finalize and Test

1. [ ] **Update project metadata**
   - [ ] Update the title, description, and other metadata in `src/app/layout.tsx`

2. [ ] **Add responsive styling improvements**
   - [ ] Test all pages on different screen sizes
   - [ ] Ensure proper spacing and layout on mobile devices

3. [ ] **Test blog functionality**
   - [ ] Verify that the blog listing page shows all published posts
   - [ ] Verify that the blog post detail page correctly renders markdown content
   - [ ] Ensure that images load correctly
   - [ ] Check that categories are displayed correctly

4. [ ] **Deploy the blog**
   - [ ] Deploy to Vercel or another hosting platform that supports Next.js
   - [ ] Set up the environment variables on the hosting platform