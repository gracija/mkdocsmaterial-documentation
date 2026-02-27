# API Reference

This section provides a detailed reference for the GitHub Repository Management API.  
It includes endpoint documentation, request/response examples, and instructions to try endpoints interactively via Swagger UI.

> Tip: Use the Swagger UI for live testing of endpoints. You can authorize with your personal access token and execute requests directly from the browser.

---

## Authentication

All endpoints require authentication via a GitHub personal access token:

- Include the token in the `Authorization` header:  
  `Authorization: Bearer <YOUR_GITHUB_TOKEN>`
- Required scopes depend on the operation (e.g., `repo` for private repositories)

---

===  "Repositories Endpoints"

    ## List User Repositories
    
    ??? note "**GET /user/repos**"

        Retrieve all repositories for the authenticated user.

        ### Request

        ```http
        GET /user/repos  
        Authorization: Bearer <YOUR_GITHUB_TOKEN>  
        Accept: application/vnd.github+json  
        ```

        ### Query Parameters

        - `type` (string) – `all` | `owner` | `public` | `private`
        - `sort` (string) – `created` | `updated` | `pushed` | `full_name`
        - `per_page` (integer) – number of items per page (default 30)
        - `page` (integer) – page number for pagination


        ### Response Example

        ```json
        [
            {
                "id": 1296269,
                "name": "Hello-World",
                "full_name": "octocat/Hello-World",
                "private": false,
                "html_url": "https://github.com/octocat/Hello-World",
                "owner": {
                "login": "octocat",
                "id": 1
                }
            }
        ]
        ```


    ## Create a Repository

    ??? note "**POST /user/repos**"

        Create a new repository for the authenticated user.

        ### Request

        ```http
        POST /user/repos  
        Authorization: Bearer <YOUR_GITHUB_TOKEN>  
        Accept: application/vnd.github+json  
        Content-Type: application/json  
        ```

        ### Query Parameters

        None.

        ### Request Body Example

        ```json
        {
        "name": "my-new-repo",
        "description": "A short description",
        "private": false,
        "auto_init": true
        }
        ```

        ### Response Example

        ```json
        {
        "id": 1296270,
        "name": "my-new-repo",
        "full_name": "octocat/my-new-repo",
        "private": false,
        "html_url": "https://github.com/octocat/my-new-repo"
        }
        ```


    ## Get Repository Details

    ??? note "**GET /repos/{owner}/{repo}**"

        Retrieve notermation about a specific repository.

        ### Request

        ```http
        GET /repos/{owner}/{repo}  
        Authorization: Bearer <YOUR_GITHUB_TOKEN>  
        Accept: application/vnd.github+json  
        ```

        ### Path Parameters

        - `owner` – repository owner username
        - `repo` – repository name

        ### Query Parameters

        None.

        ### Response Example

        ```json
        {
        "id": 1296269,
        "name": "Hello-World",
        "full_name": "octocat/Hello-World",
        "private": false,
        "html_url": "https://github.com/octocat/Hello-World",
        "description": "This is your first repo!"
        }
        ```


=== "Issues Endpoints"

    ## List Issues in a Repository

    ??? note "**GET /repos/{owner}/{repo}/issues**"

        Retrieve all issues for a repository.

        ### Request

        ```http
        GET /repos/{owner}/{repo}/issues  
        Authorization: Bearer <YOUR_GITHUB_TOKEN>  
        Accept: application/vnd.github+json  
        ```

        ### Path Parameters

        - `owner` – repository owner username
        - `repo` – repository name

        ### Query Parameters

        - `state` (string) – `open` | `closed` | `all`
        - `labels` (string) – comma-separated label names
        - `assignee` (string) – username of assignee
        - `per_page` (integer) – number of items per page
        - `page` (integer) – page number for pagination

        ### Response Example

        ```json
        [
            {
                "id": 1,
                "number": 42,
                "title": "Bug: Fix login issue",
                "state": "open",
                "assignee": { "login": "octocat" },
                "labels": [{ "name": "bug" }]
            }
        ]
        ```


    ## Create an Issue

    ??? note "**POST /repos/{owner}/{repo}/issues**"

        Create a new issue in a repository.

        ### Request

        ```http
        POST /repos/{owner}/{repo}/issues  
        Authorization: Bearer <YOUR_GITHUB_TOKEN>  
        Accept: application/vnd.github+json  
        Content-Type: application/json  
        ```

        ### Path Parameters

        - `owner` – repository owner username
        - `repo` – repository name

        ### Query Parameters

        None.

        ### Request Body Example

        ```json
        {
        "title": "New Feature Request",
        "body": "Please add support for dark mode.",
        "assignees": ["octocat"],
        "labels": ["enhancement"]
        }
        ```

        ### Response Example

        ```json
        {
        "id": 101,
        "number": 101,
        "title": "New Feature Request",
        "state": "open"
        }
        ```

=== "Pull Requests Endpoints"

    ## Create a Pull Request

    ??? note "**POST /repos/{owner}/{repo}/pulls**"

        Create a new pull request in a repository.

        ### Request

        ```http
        POST /repos/{owner}/{repo}/pulls  
        Authorization: Bearer <YOUR_GITHUB_TOKEN>  
        Accept: application/vnd.github+json  
        Content-Type: application/json  
        ```

        ### Path Parameters

        - `owner` – repository owner username
        - `repo` – repository name

        ### Query Parameters

        None.

        ### Request Body Example

        ```json
        {
        "title": "Add login validation",
        "head": "feature/login-fix",
        "base": "main",
        "body": "This PR fixes validation issues in the login form."
        }
        ```

        ### Response Example

        ```json
        {
        "id": 108,
        "number": 108,
        "state": "open",
        "title": "Add login validation",
        "body": "This PR fixes validation issues in the login form."
        }
        ```

---

## Try It in Swagger

All endpoints are interactive in the Swagger UI:

1. Open the _Swagger UI_ page from the menu on the left
2. Click Authorize and enter your personal access token  
3. Explore endpoints and execute requests directly  
4. Inspect live JSON responses  

This ensures you can test workflows without writing any code.