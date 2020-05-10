# Migration `20200508203644-add-plans`

This migration has been generated by fmvilas at 5/8/2020, 8:36:44 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "public"."organizations" ALTER COLUMN "plan_name" SET DEFAULT 'free';

ALTER TABLE "public"."users" ALTER COLUMN "plan_name" SET DEFAULT 'free';
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200506152925-initial..20200508203644-add-plans
--- datamodel.dml
+++ datamodel.dml
@@ -3,9 +3,9 @@
 }
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url      = env("DATABASE_URL")
 }
 model apis {
   asyncapi          String
@@ -38,12 +38,14 @@
   featureFlags                Json?                 @map(name: "feature_flags")
   id                          Int                   @default(autoincrement()) @id
   name                        String
   creatorId                   Int                   @map(name: "creator_id")
+  planName                    String?               @default("free") @map(name: "plan_name")
   invitations                 invitations[]
   organizationUsers           organizationsUsers[]
   projects                    projects[]
   creator                     users                 @relation(fields: [creatorId], references: [id])
+  plan                        plans?                @relation(fields: [planName], references: [name])
 }
 model organizationsUsers {
   createdAt       DateTime?     @default(now()) @map(name: "created_at")
@@ -76,10 +78,17 @@
   email                String                @unique
   featureFlags         Json?                 @map(name: "feature_flags")
   id                   Int                   @default(autoincrement()) @id
   username             String                @unique
+  planName             String?               @default("free") @map(name: "plan_name")
   apis                 apis[]
   invitations          invitations[]
   organizationsForUser organizationsUsers[]
   projects             projects[]
   organizations        organizations[]
+  plan                 plans?                @relation(fields: [planName], references: [name])
 }
+
+model plans {
+  name         String @unique
+  restrictions Json?
+}
```

