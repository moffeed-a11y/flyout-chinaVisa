# ניהול ויזה לסין 🇨🇳

מערכת מקצועית לניהול לקוחות ויזה. הרשמה + התחברות, נתונים בענן, ותמונות באחסון אמיתי (Supabase).

## הקמה חד-פעמית ב-Supabase

### 1. מסד נתונים
SQL Editor → הרץ:
```sql
create table clients (
  id text primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);
alter table clients enable row level security;
create policy "auth all" on clients
  for all to authenticated using (true) with check (true);
grant all on table clients to authenticated;
```

### 2. אחסון תמונות (Storage)
SQL Editor → הרץ:
```sql
insert into storage.buckets (id, name, public)
values ('photos','photos', true) on conflict (id) do nothing;

create policy "photos write" on storage.objects
  for insert to authenticated with check (bucket_id = 'photos');
create policy "photos update" on storage.objects
  for update to authenticated using (bucket_id = 'photos');
create policy "photos read" on storage.objects
  for select to public using (bucket_id = 'photos');
```

### 3. הרשמה חלקה
Authentication → Providers → Email → כבה **Confirm email**
(כדי שההרשמה מהאתר תיכנס ישר בלי אימות מייל)

### 4. מפתחות
Project Settings → API → העתק **Project URL** + **anon public key**

## הפעלה
1. פתח את האתר → הדבק URL + key
2. "צור חשבון חדש" → הירשם עם אימייל + סיסמה
3. מעכשיו הנתונים והתמונות בענן, נגישים מכל מכשיר

## אבטחה
אחרי שיצרת את החשבון שלך — כבה הרשמות חדשות:
Authentication → Providers → Email → **Allow new users to sign up** = OFF

## תכונות
נוסעים לפי שלבים · סריקת דרכון · תשלומים · תמונות · הדפסת כרטיס · מסך סטטוס ללקוח + וואטסאפ · עברית/ערבית · אקסל

## הערה
בתוכנית החינמית, פרויקט ללא שימוש 7 ימים נכנס לשינה (הנתונים נשמרים).

## אתר חי
https://USERNAME.github.io/china-visa/
