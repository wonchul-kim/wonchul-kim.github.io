---
layout: post
title: Create Table in pgadmin
category: Backend
tag: [pgadmin, sql]
---

### Using query tool, 

CREATE TABLE "PatchInfoDB" (
    "id" SERIAL PRIMARY KEY,
    "img_folder" VARCHAR(100) NOT NULL,
    "classes" VARCHAR(100) NOT NULL,
    "roi_info" JSONB NOT NULL,
    "patch_info" JSONB NOT NULL,
    "imgs_info" JSONB NOT NULL,
    "total_num_imgs" INTEGER NOT NULL,
    "avg_num_rois" INTEGER NOT NULL,
    "avg_num_backgrounds" INTEGER NOT NULL,
    "created_at" TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
