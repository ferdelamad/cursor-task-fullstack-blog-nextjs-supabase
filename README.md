# Next.js and Supabase Blog Template

A fullstack blog template built with Next.js and Supabase.

## Features

- ⚡️ Latest Next.js version and configuration
- 💾 Supabase for database for storage
- 🎨 Tailwind CSS for styling and Shadcn UI for components
- 📱 Responsive design
- 🔍 SEO optimized

## Getting Started

### Prerequisites

- Node.js 20+ (specified in .nvmrc)
- Supabase account

### Installation

1. Clone this repository
2. Run the initial setup script:
   ```bash
   nvm use

   chmod +x setup.sh

   ./setup.sh
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```

### Implementing with Cursor

This template is designed to work with the Cursor IDE and its AI agent (composer). To get started:

1. **Run with Cursor Composer**
   - Open Cursor IDE
   - Press `Cmd + I` (Mac) or `Ctrl + I` (Windows/Linux) to open the Cursor Composer
     - modify the chat mode to 'Agent': ![screenshot](./agent_toggle.png)
   - Add both `.cursor/rules/base.mdc` and `.cursor/tasks/init.md` to the composer context
   - Use the following prompt:
     ```
     Go through each task in the init.md file. After you complete each task,
     update the file to check off any task. Run builds and commits after each task.
     Continue with each task until you have checked off each one. After each story,
     do not take a screenshot. If you need more detail about a task, you can gather
     relevant files and pass the FULL file to the research agent.
     ```

2. **Monitor Progress**
   - The agent will work through each task in order
   - Tasks will be checked off automatically as they're completed
   - Commits will be made after each task
   - You can monitor progress in the cursor-tasks.md file

## Ideal Project Structure

```
src/
├── app/                 # Next.js App Router
│   ├── blog/            # Blog routes
│   ├── api/             # API routes
├── components/          # React components
│   ├── ui/              # Reusable UI components
│   ├── blog/            # Blog-specific components
├── lib/                 # Utility functions
│   ├── supabase.ts      # Supabase client
│   ├── utils.ts         # Utility functions
│   ├── api.ts           # API helpers
│   ├── database.types.ts # Supabase database types
```

## Database Schema

The template uses the following database schema in Supabase:

### Categories Table

| Column     | Type                    | Description                    |
|------------|-------------------------|--------------------------------|
| id         | UUID                    | Primary key (auto-generated)   |
| name       | TEXT                    | Category name                  |
| slug       | TEXT                    | Unique URL-friendly identifier |
| created_at | TIMESTAMP WITH TIME ZONE| Creation timestamp             |

### Posts Table

| Column         | Type                    | Description                         |
|---------------|-------------------------|--------------------------------------|
| id            | UUID                    | Primary key (auto-generated)         |
| title         | TEXT                    | Post title                           |
| slug          | TEXT                    | Unique URL-friendly identifier       |
| content       | TEXT                    | Main post content                    |
| featured_image| TEXT                    | URL to featured image                |
| excerpt       | TEXT                    | Short post summary                   |
| category_id   | UUID                    | Foreign key to categories table      |
| published     | BOOLEAN                 | Publication status                   |
| published_at  | TIMESTAMP WITH TIME ZONE| Publication timestamp                |
| created_at    | TIMESTAMP WITH TIME ZONE| Creation timestamp                   |
| updated_at    | TIMESTAMP WITH TIME ZONE| Last update timestamp                |

## Deployment

This project can be deployed to Vercel:

```bash
npm run build
npm run start
```

## License

MIT