# Mister-T Home

This is the personal blog of 阿土(Mr.T).

## Getting Started

Follow these instructions to set up the project locally for development and testing.

### Prerequisites

- [Git](https://git-scm.com/)
- [Hugo](https://gohugo.io/getting-started/installing/) (extended version)

### Installation

1.  Clone the repository and its theme submodule:

    ```bash
    git clone --recurse-submodules https://github.com/wayne6172/wayne6172.github.io.git
    ```

2.  Navigate to the project directory:

    ```bash
    cd wayne6172.github.io
    ```

3.  Start the Hugo development server:

    ```bash
    hugo server -D
    ```

4.  Open your browser and navigate to `http://localhost:1313/` to see the blog.

## Creating a new post

To create a new blog post, use the following command:

```bash
hugo new content posts/your-post-title.md
```

This will create a new Markdown file in the `content/posts` directory with a pre-filled front matter. You can then edit this file to add your content.
