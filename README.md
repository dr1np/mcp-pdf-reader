# 📄 MCP PDF Server

A PDF file reading server based on [FastMCP](https://github.com/jlowin/fastmcp).

Supports PDF text extraction, OCR recognition, and image extraction via the MCP protocol, with a built-in web debugger for easy testing.

---

## 🚀 Features

- **read_pdf_text**  
  Extracts normal text from a PDF (page by page).

- **read_by_ocr**  
  Uses OCR to recognize text from scanned or image-based PDFs.

- **read_pdf_images**  
  Extracts all images from a specified PDF page (Base64 encoded output).

---

## 📂 Project Structure

```text
mcp-pdf-reader/
├── pdf_resources/        # Directory for uploaded and processed PDF files
├── pdf_server.py         # Main server entry point
├── pyproject.toml        # Project configuration and dependencies
└── README.md             # Project documentation
```

---

## ⚙️ Installation

Recommended Python version: 3.10+

using `uv`:

```bash
uv sync
```

> Note: To use OCR features, you may need a MuPDF build with OCR support or external OCR libraries.

---

## 🔦 Start the Server

Run the following command:

```bash
python pdf_server.py
```

You should see logs like:

```text
INFO:mcp-pdf-server:Starting MCP PDF Server...
INFO:mcp-pdf-server:PDF resources will be stored in: C:\...\pdf_resources
```

The server will start and expose MCP tools via stdio protocol.

If you want to use this tool in Claude Desktop, config this JSON.

```json
{
  "mcpServers": {
    "pdf-reader-custom": {
      "command": "uv",
      "args": [
         "--directory",
         "path/to/your/pdf-mcp",
         "run",
         "pdf_server.py"
      ]
    }
  }
}
```

---


## 🛠️ API Tool List

| Tool | Description | Input Parameters | Returns |
|:-----|:------------|:-----------------|:--------|
| `read_pdf_text` | Extracts normal text from PDF pages | `file_path`, `start_page`, `end_page` | List of page texts |
| `read_by_ocr` | Recognizes text via OCR | `file_path`, `start_page`, `end_page`, `language`, `dpi` | OCR extracted text |
| `read_pdf_images` | Extracts images from a PDF page | `file_path`, `page_number` | List of images (Base64 encoded) |

---

## 📝 Example Usage

This is an MCP server that exposes tools to be called by MCP clients. The tools can be integrated into:

- **Claude Desktop** via MCP configuration
- **LLM applications** using the MCP protocol
- **Custom scripts** using MCP client libraries

Example parameters for each tool:

**Extract text from pages 1 to 5:**

```json
{
  "file_path": "pdf_resources/example.pdf",
  "start_page": 1,
  "end_page": 5
}
```

**Perform OCR recognition on page 1:**

```json
{
  "file_path": "pdf_resources/example.pdf",
  "start_page": 1,
  "end_page": 1,
  "language": "eng",
  "dpi": 300
}
```

**Extract all images from page 3:**

```json
{
  "file_path": "pdf_resources/example.pdf",
  "page_number": 3
}
```

### Integration with Claude Desktop

Add to your Claude Desktop config:

```json
{
  "mcpServers": {
    "pdf-reader": {
      "command": "python",
      "args": ["/path/to/pdf_server.py"]
    }
  }
}
```

---

## 📢 Notes

- Files must be placed inside the `pdf_resources/` directory, or an absolute path must be provided.
- OCR functionality requires appropriate OCR support in the environment.
- When processing large files, adjust memory and timeout settings as needed.

---

## 📜 License

This project is licensed under the MIT License.  
For commercial use, please credit the original source.
