# JEdenManga
Unofficial Manga Eden API client for java see more at https://www.mangaeden.com/api

~~Currently this api has limited cunctionality, we wont be able to update the progress with  manga eden.~~

**Manga List**
```java
try {
  JEdenManga edenManga = new JEdenManga();
  // edenManga.setLanguage(Language.Italian);
  edenManga.setLanguage(Language.English);
  MangaList allManga	 = edenManga.getAllManga();
} catch (IOException e) {
	e.printStackTrace();
}
```
Where language can be English or Italian, default is English

Returned data:
MangaListObject with 
- page,start,end, and, total
- list of "mangas" which contains the manga's image, title, ID, alias, status, category (genre), last chapter date , hits, 
___
**Manga List splitted in pages**

Same as above but returns only 500 manga's informations (from manga X*500 to (X+1)*500, where X is the page argument)
```java
try {
  JEdenManga edenManga = new JEdenManga();
  MangaList first500Manga = edenManga.getMangaListSplited(0);
  List<Manga> mangas = first500Manga.getMangas();
 } catch (IOException e) {
  e.printStackTrace();
} 
  ```
 ___
 
**Manga List splitted in pages with variable page size**

Same as above but returns only Y manga's informations (from manga X*Y to (X+1)*Y) [25 < Y < 1500]
```java
try {
 JEdenManga edenManga = new JEdenManga();
 MangaList first10Manga = edenManga.getMangaListSplitedRange(0, 10);
 List<Manga> mangas = first10Manga.getMangas();
} catch (IOException e) {
 e.printStackTrace();
}
```
 ___
**Manga info and chapters list**
```java
try {
 JEdenManga edenManga = new JEdenManga();
 MangaInfo mangaInfo = edenManga.getMangaInfo("managId.");
} catch (IOException e) {
 e.printStackTrace();
}
	
	//OR
	
try {
 JEdenManga edenManga = new JEdenManga();
 MangaList first10Manga = edenManga.getMangaListSplitedRange(0, 10);
 List<Manga> mangas = first10Manga.getMangas();
 MangaInfo mangaInfo = mangas.get(0).getMangaInfo();
} catch (IOException e) {
 e.printStackTrace();
}
```
Where mangaId is the manga's id you can get with the previous api call
Returned data: all the informations and chapters of the manga.
___
**Chapter pages**

```java
try {
 JEdenManga edenManga = new JEdenManga();
 // edenManga.setLanguage(Language.Italian);
 edenManga.setLanguage(Language.English);
 ArrayList<Page> chapterPages = edenManga.getChapterPages("chapterId");
} catch (IOException e) {
 e.printStackTrace();
}
	
	//OR
	
try {
 JEdenManga edenManga = new JEdenManga();
 // edenManga.setLanguage(Language.Italian);
 edenManga.setLanguage(Language.English);
 MangaList first10Manga = edenManga.getMangaListSplitedRange(0, 10);
 List<Manga> mangas = first10Manga.getMangas();
 MangaInfo mangaInfo = mangas.get(0).getMangaInfo();
 ArrayList<ChapterInfo> chapterPages = mangaInfo.getChapters();
 ArrayList<Page> pages = chapterPages.get(0).getPages();
} catch (IOException e) {
 e.printStackTrace();
}
```
Where [chapterId] is the chapter's id you can get with the previous api call.
Returned data: the images's urls and sizes of the chapter

___
**Login**
```java
 JEdenManga edenManga = new JEdenManga();
 Status status = edenManga.login("userName", "pasword".toCharArray());
 System.out.println(status==Status.OK);
 System.out.println(status==Status.ERROR);
```
password should be parsed as char array
returns Status.OK when successful, Status.ERROR on failure

___
**Logout**
```java
 JEdenManga edenManga = new JEdenManga();
 Status status = edenManga.logout()
 System.out.println(status==Status.OK);
 System.out.println(status==Status.ERROR);
```
password should be parsed as char array
returns Status.OK when successful, Status.ERROR on failure

___

**MyManga**

User should login before calling this api
Returns all manga saved in the user's mymanga list with all the informations about the manga, latest available chapter and last chapter read by the user.
```java
 JEdenManga edenManga = new JEdenManga();
 Status status = edenManga.login("userName", "pasword".toCharArray());
 if(status==Status.OK) {
 	List<MyManga> myManga = edenManga.myManga();
 	myManga.forEach(manga->{
 		System.out.println(manga.getLastChapterRead());
 	});
 }
```