<!--
➜  three_models git:(main) ✗ rails c
Loading development environment (Rails 7.1.2)
[1] pry(main)> Blog.all
  Blog Load (0.1ms)  SELECT "blogs".* FROM "blogs"
+----+--------------------------------+-------------------------+-------------------------+
| id | title                          | created_at              | updated_at              |
+----+--------------------------------+-------------------------+-------------------------+
| 1  | ねこがすき！にゃんにゃんブログ | 2024-01-16 13:47:32 UTC | 2024-01-16 13:47:32 UTC |
| 2  | いぬがすき！わんわんブログ     | 2024-01-16 13:47:56 UTC | 2024-01-16 13:47:56 UTC |
| 3  | つまがすき！いとうさんブログ   | 2024-01-16 13:48:10 UTC | 2024-01-16 13:48:10 UTC |
+----+--------------------------------+-------------------------+-------------------------+
3 rows in set
[2] pry(main)> Entry.all
  Entry Load (0.2ms)  SELECT "entries".* FROM "entries"
+----+----------------------+------------------------+---------+-------------------------+-------------------------+
| id | title                | body                   | blog_id | created_at              | updated_at              |
+----+----------------------+------------------------+---------+-------------------------+-------------------------+
| 1  | はじめてのエントリー | はじめまして！         | 1       | 2024-01-16 13:50:21 UTC | 2024-01-16 13:50:21 UTC |
| 2  | 2番目のエントリー    | おひさしぶりです！     | 1       | 2024-01-16 14:00:05 UTC | 2024-01-16 14:00:05 UTC |
| 3  | 3番目のエントリー    | もうくじけました・・・ | 3       | 2024-01-16 14:01:01 UTC | 2024-01-16 14:01:01 UTC |
+----+----------------------+------------------------+---------+-------------------------+-------------------------+
3 rows in set
[3] pry(main)> Comment.all
  Comment Load (0.3ms)  SELECT "comments".* FROM "comments"
+----+------------------------+------------+----------+-------------------------+-------------------------+
| id | body                   | status     | entry_id | created_at              | updated_at              |
+----+------------------------+------------+----------+-------------------------+-------------------------+
| 1  | てすてす               | approved   | 1        | 2024-01-16 14:04:40 UTC | 2024-01-16 14:04:40 UTC |
| 2  | ねこはかわいいですよね | unapproved | 1        | 2024-01-16 14:05:11 UTC | 2024-01-16 14:05:11 UTC |
| 3  | 例のねこについて       | approved   | 2        | 2024-01-16 14:05:42 UTC | 2024-01-16 14:05:42 UTC |
| 4  | こんにちはこんにちは！ | approved   | 3        | 2024-01-16 14:06:04 UTC | 2024-01-16 14:06:04 UTC |
+----+------------------------+------------+----------+-------------------------+-------------------------+
4 rows in set

# Blogのid:1に紐づくすべてのCommentを表示してください。
[4] pry(main)> Comment.joins(:entry).where(entries: { blog_id: 1 })
  Comment Load (0.1ms)  SELECT "comments".* FROM "comments" INNER JOIN "entries" ON "entries"."id" = "comments"."entry_id" WHERE "entries"."blog_id" = ?  [["blog_id", 1]]
+----+------------------------+------------+----------+-------------------------+-------------------------+
| id | body                   | status     | entry_id | created_at              | updated_at              |
+----+------------------------+------------+----------+-------------------------+-------------------------+
| 1  | てすてす               | approved   | 1        | 2024-01-16 14:04:40 UTC | 2024-01-16 14:04:40 UTC |
| 2  | ねこはかわいいですよね | unapproved | 1        | 2024-01-16 14:05:11 UTC | 2024-01-16 14:05:11 UTC |
| 3  | 例のねこについて       | approved   | 2        | 2024-01-16 14:05:42 UTC | 2024-01-16 14:05:42 UTC |
+----+------------------------+------------+----------+-------------------------+-------------------------+
3 rows in set

# まだEntryを書いていないBlogを表示してください。
[6] pry(main)> Blog.left_outer_joins(:entries).where(entries: { id: nil })
  Blog Load (0.3ms)  SELECT "blogs".* FROM "blogs" LEFT OUTER JOIN "entries" ON "entries"."blog_id" = "blogs"."id" WHERE "entries"."id" IS NULL
+----+----------------------------+-------------------------+-------------------------+
| id | title                      | created_at              | updated_at              |
+----+----------------------------+-------------------------+-------------------------+
| 2  | いぬがすき！わんわんブログ | 2024-01-16 13:47:56 UTC | 2024-01-16 13:47:56 UTC |
+----+----------------------------+-------------------------+-------------------------+
1 row in set

# statusがunapprovedであるCommentがあるEntryのあるBlogを表示してください。
[7] pry(main)> Blog.joins(entries: :comments).where(comments: { status: 'unapproved' }).distinct
  Blog Load (0.2ms)  SELECT DISTINCT "blogs".* FROM "blogs" INNER JOIN "entries" ON "entries"."blog_id" = "blogs"."id" INNER JOIN "comments" ON "comments"."entry_id" = "entries"."id" WHERE "comments"."status" = ?  [["status", "unapproved"]]
+----+--------------------------------+-------------------------+-------------------------+
| id | title                          | created_at              | updated_at              |
+----+--------------------------------+-------------------------+-------------------------+
| 1  | ねこがすき！にゃんにゃんブログ | 2024-01-16 13:47:32 UTC | 2024-01-16 13:47:32 UTC |
+----+--------------------------------+-------------------------+-------------------------+
1 row in set
-->