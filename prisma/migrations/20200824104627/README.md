# Migration `20200824104627`

This migration has been generated by Andy Kay at 8/24/2020, 10:46:27 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE `spacesafe`.`User` (
`createdAt` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ,`email` varchar(191) NOT NULL  ,`id` varchar(191) NOT NULL  ,`image` varchar(191) NOT NULL  ,`isAdmin` boolean NOT NULL DEFAULT false ,`name` varchar(191) NOT NULL  ,
    PRIMARY KEY (`id`)
) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci

CREATE TABLE `spacesafe`.`Location` (
`createdAt` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ,`fieldSite` boolean NOT NULL DEFAULT false ,`id` varchar(191) NOT NULL  ,`name` varchar(191) NOT NULL  ,`updatedAt` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ,
    PRIMARY KEY (`id`)
) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci

CREATE TABLE `spacesafe`.`ActivityLog` (
`checkIn` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ,`checkOut` datetime(3)   ,`id` varchar(191) NOT NULL  ,`locationId` varchar(191) NOT NULL ,`notes` varchar(191)   ,`roomNumber` int   ,`userId` varchar(191) NOT NULL ,
    PRIMARY KEY (`id`)
) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci

CREATE UNIQUE INDEX `User.email` ON `spacesafe`.`User`(`email`)

CREATE UNIQUE INDEX `Location.name` ON `spacesafe`.`Location`(`name`)

ALTER TABLE `spacesafe`.`ActivityLog` ADD FOREIGN KEY (`userId`) REFERENCES `spacesafe`.`User`(`id`) ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE `spacesafe`.`ActivityLog` ADD FOREIGN KEY (`locationId`) REFERENCES `spacesafe`.`Location`(`id`) ON DELETE CASCADE ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20200824104627
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,42 @@
+// This is your Prisma schema file,
+// learn more about it in the docs: https://pris.ly/d/prisma-schema
+
+datasource db {
+  provider = "mysql"
+  url      = env("DATABASE_URL")
+}
+
+generator client {
+  provider = "prisma-client-js"
+}
+
+model User {
+  id          String        @id @default(uuid())
+  name        String
+  email       String        @unique
+  image       String
+  isAdmin     Boolean       @default(false)
+  createdAt   DateTime      @default(now())
+  ActivityLog ActivityLog[]
+}
+
+model Location {
+  id          String        @id @default(uuid())
+  name        String        @unique
+  fieldSite   Boolean       @default(false)
+  createdAt   DateTime      @default(now())
+  updatedAt   DateTime      @default(now())
+  ActivityLog ActivityLog[]
+}
+
+model ActivityLog {
+  id         String    @id @default(uuid())
+  user       User      @relation(fields: [userId], references: id)
+  userId     String
+  location   Location  @relation(fields: [locationId], references: id)
+  locationId String
+  roomNumber Int?
+  notes      String?
+  checkIn    DateTime  @default(now())
+  checkOut   DateTime?
+}
```


