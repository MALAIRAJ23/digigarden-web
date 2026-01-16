# 🌿 Digital Garden

A modern, feature-rich personal knowledge management system built with Next.js and Supabase. Create, organize, and connect your notes with wiki-style linking, visualize your knowledge graph, and sync across devices.

![Digital Garden](https://img.shields.io/badge/Next.js-13+-black?style=flat-square&logo=next.js)
![React](https://img.shields.io/badge/React-18+-blue?style=flat-square&logo=react)
![Supabase](https://img.shields.io/badge/Supabase-Enabled-green?style=flat-square&logo=supabase)

## ✨ Features

### 📝 Note Management
- **Markdown Editor** with live preview and syntax highlighting
- **Wiki-style Linking** using `[[Note Title]]` syntax
- **Auto-save** functionality (every 3 seconds)
- **Version History** tracking
- **Tags & Collections** for organization
- **Public/Private** visibility settings
- **Favorites** system

### 🔍 Discovery & Search
- **Fuzzy Search** across all notes
- **Tag Filtering** and advanced sorting
- **Random Discovery** for serendipitous connections
- **Similar Notes** recommendations
- **Backlinks** visualization

### 📊 Analytics & Insights
- **Writing Statistics** (word count, notes count, reading time)
- **Writing Streaks** tracking
- **Activity Dashboard** with customizable widgets
- **Top Tags** analysis
- **Most Linked Notes** insights

### 🕸️ Knowledge Graph
- **Interactive Network Visualization** of note connections
- **Visual Representation** of wiki-links
- **Double-click Navigation** to notes

### 🎨 Customization
- **Dark/Light/System** theme modes
- **Theme Customizer** with color presets
- **Responsive Design** (mobile, tablet, desktop)
- **Keyboard Shortcuts** for power users
- **Command Palette** (Ctrl+K / Cmd+K)

### ☁️ Cloud Integration
- **Supabase Authentication** (email/password)
- **Cloud Storage** with automatic sync
- **User-specific Data Isolation** with Row Level Security
- **localStorage Fallback** for offline use
- **One-click Migration** from local to cloud

### 📤 Import/Export
- **JSON Export** of all notes
- **File Import** (text, markdown)
- **Drag-and-Drop** file uploads
- **Note Reordering** with drag-and-drop

## 🚀 Getting Started

### Prerequisites

- Node.js 16+ and npm/yarn
- Supabase account (free tier works)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/digital-garden.git
cd digital-garden/digigarden
```

2. **Install dependencies**
```bash
npm install
# or
yarn install
```

3. **Set up Supabase**

Create a new project at [supabase.com](https://supabase.com)

4. **Configure environment variables**

Create `.env.local` in the root directory:

```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

5. **Set up database tables**

Run these SQL commands in your Supabase SQL editor:

```sql
-- Notes table
CREATE TABLE notes (
  id BIGSERIAL PRIMARY KEY,
  user_id UUID REFERENCES auth.users NOT NULL,
  title TEXT NOT NULL,
  content TEXT,
  slug TEXT,
  tags TEXT[],
  visibility TEXT DEFAULT 'private',
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Collections table
CREATE TABLE collections (
  id BIGSERIAL PRIMARY KEY,
  user_id UUID REFERENCES auth.users NOT NULL,
  name TEXT NOT NULL,
  type TEXT,
  query JSONB,
  color TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Favorites table
CREATE TABLE favorites (
  id BIGSERIAL PRIMARY KEY,
  user_id UUID REFERENCES auth.users NOT NULL,
  note_id BIGINT REFERENCES notes(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(user_id, note_id)
);

-- Enable Row Level Security
ALTER TABLE notes ENABLE ROW LEVEL SECURITY;
ALTER TABLE collections ENABLE ROW LEVEL SECURITY;
ALTER TABLE favorites ENABLE ROW LEVEL SECURITY;

-- RLS Policies for notes
CREATE POLICY "Users can view their own notes"
  ON notes FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own notes"
  ON notes FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update their own notes"
  ON notes FOR UPDATE
  USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own notes"
  ON notes FOR DELETE
  USING (auth.uid() = user_id);

-- RLS Policies for collections
CREATE POLICY "Users can view their own collections"
  ON collections FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own collections"
  ON collections FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update their own collections"
  ON collections FOR UPDATE
  USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own collections"
  ON collections FOR DELETE
  USING (auth.uid() = user_id);

-- RLS Policies for favorites
CREATE POLICY "Users can view their own favorites"
  ON favorites FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own favorites"
  ON favorites FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can delete their own favorites"
  ON favorites FOR DELETE
  USING (auth.uid() = user_id);
```

6. **Run the development server**
```bash
npm run dev
# or
yarn dev
```

7. **Open your browser**

Navigate to [http://localhost:3000](http://localhost:3000)

## 📖 Usage

### Creating Notes
1. Click "Create New Note" or press `Ctrl+N`
2. Write in Markdown with live preview
3. Add tags and set visibility
4. Auto-saves every 3 seconds

### Linking Notes
Use wiki-style syntax: `[[Note Title]]`
- Type `[[` to see suggestions
- Creates bidirectional links
- View backlinks on each note

### Keyboard Shortcuts
- `Ctrl+K` / `Cmd+K` - Command Palette
- `Ctrl+S` / `Cmd+S` - Save Note
- `Ctrl+B` / `Cmd+B` - Bold Text
- `Ctrl+I` / `Cmd+I` - Italic Text
- `?` - Show all shortcuts

### Collections
- **Smart Collections**: Auto-update based on tags/dates
- **Manual Collections**: Curate specific notes
- **Default Collections**: Favorites, Recent, Work, Learning

## 🏗️ Tech Stack

- **Framework**: Next.js 13+
- **UI Library**: React 18+
- **Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **Styling**: Custom CSS with design system
- **Markdown**: ReactMarkdown + remark-gfm
- **Graph Visualization**: vis-network
- **Deployment**: Vercel-ready

## 📁 Project Structure

```
digigarden/
├── components/          # React components
│   ├── editor.jsx      # Markdown editor
│   ├── ErrorBoundary.jsx
│   ├── LoadingState.jsx
│   └── ...
├── contexts/           # React contexts
│   └── AuthContext.jsx
├── lib/                # Utility functions
│   ├── storage.js      # Database operations
│   ├── analytics.js    # Analytics calculations
│   └── ...
├── pages/              # Next.js pages
│   ├── index.jsx       # Home page
│   ├── dashboard.jsx   # Dashboard
│   ├── notes/          # Note pages
│   └── ...
├── styles/
│   └── globals.css     # Global styles
└── public/             # Static assets
```

## 🔒 Security

- Row Level Security (RLS) policies ensure data isolation
- User authentication required for cloud features
- localStorage fallback for offline/anonymous use
- No sensitive data in client-side code

## 🚢 Deployment

### Vercel (Recommended)

1. Push code to GitHub
2. Import project in Vercel
3. Add environment variables
4. Deploy!

### Other Platforms

Works with any platform supporting Next.js:
- Netlify
- Railway
- AWS Amplify
- Self-hosted

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📄 License

MIT License - feel free to use for personal or commercial projects

## 🙏 Acknowledgments

- Inspired by Obsidian, Roam Research, and Notion
- Built with amazing open-source tools
- Community feedback and contributions

## 📧 Contact

Questions? Issues? Reach out or open an issue on GitHub!

---

**Made with ❤️ for knowledge workers, students, and lifelong learners**
"# digigarden-web" 
