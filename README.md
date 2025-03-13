# Ansible Collection Development and Deployment Workflow

This document outlines the workflow for developing, reviewing, and deploying Ansible collections within Red Hat’s internal Ansible Library.

## 🚀 Workflow Overview

1. **Consultant Creates a Personal Branch and Develops a Collection**  
   - Consultant creates a **personal branch** off of `staging`.
        -  ``` git checkout -b my-feature-branch origin/staging ```
   - Develop a new collection or update an existing one.
   - Choose the Appropriate Namespace
        - Before creating a new collection, check the existing namespaces under the `namespaces/` directory.  
        - If you believe a new namespace is required, reach out to the **Ansible Library Admins** for approval.
    - Initialize the Collection Structure
        -  ```ansible-galaxy collection init my_namespace.my_collection```
    - Navigate to the Collection Directory
        - ``` cd my_namespace/my_collection```
    - Add Roles, Plugins, and Modules
        - Place roles in `roles/`
        - Add plugins in `plugins/`
        - Define modules in `modules/`
        - Include documentation in `docs/`
    - Define Collection Metadata
        - Edit the galaxy.yml file and ensure it includes relevant metadata such as:
        ``` yaml
        namespace: my_namespace
        name: my_collection
        version: 1.0.0 # If updating an existing collection please increment the version to not cause any breaks
        description: A brief description of the collection.
        ```
   - When ready, **push changes** (this automatically syncs the branch with `staging`).
        ```
        git add .
        git commit -m "Initialized new collection my_namespace.my_collection"
        git push origin my-feature-branch
        ```
   - *TODO: CICD pipeline should include the following:*
        - *ansible-lint to enforce best practices.*
        - *A customer data scrubber to prevent sensitive data leaks.*

2. **Ansible Library Admins Review Changes and Merge To Main**  
   - **Ansible Library Admins** manually create a **pull request** from `staging` to `main`.
   - Admins review the incoming changes and **approve or request modifications**.
   - Incoming changes are reviewed manually for quality and security.

3. **Sync GitHub (Public) and GitLab (Private)**  
   - CI/CD **syncs GitHub** (public) → **GitLab** (private) to ensure they  remain synchronized.
   - CI/CD **pushes new changes** from **GitLab to GitHub** after approval.

4. **Automated Collection Build & Publish**  
   - On the **1st of every month** (or manually triggered), GitHub CI/CD pipeline:
     - Builds **only new or updated** collections.
     - Publishes them to **[Private Automation Hub](https://platform.cus-l3n9so.aws.ansiblecloud.redhat.com/content/collections?page=1&perPage=10&sort=name)**.

5. **Collection Approval in Private Automation Hub**  
   - **Ansible Library Admins** review and approve the new collection.
   - Once approved, the collection becomes **available to Red Hat consultants**.

---

## 🔄 CI/CD Automation Breakdown

| Step | Trigger | Action |
|------|---------|--------|
| **Personal Branch Sync** | Consultant push to personal branch | Auto-merges to `staging` |
| **Staging to Main Review** | Manual PR creation | Admins review and approve |
| **GitHub ↔ GitLab Sync** | New changes in `main` | Ensures public/private parity |
| **Collection Build & Publish** | Monthly/Manual Trigger | Publishes updated collections |
| **Automation Hub Approval** | Manual Admin Review | Makes collection available |

---

## 🔧 Future Enhancements
- Add **Ansible-linter** for code validation before syncing to `staging`.
- Implement a **customer data scrubber** to prevent sensitive information in collections.
- Introduce **Slack notification alerts** when collections are ready for review in Automation Hub.

---

## 📞 Contact Us

If you have any questions, need assistance, or require a new namespace, please reach out to the **Ansible Library Admins**:

| Name            | Email Address           |
|----------------|------------------------|
| Alex Benavides | abenavid@redhat.com  |
| John Best      | jbest@redhat.com     |
| Walter Bentley | wbentley@redhat.com  |

For general inquiries, you can also reach us via the **Slack**