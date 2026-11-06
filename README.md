# Лаборатори №8: Mock объектууд (Mockito)

**Оюутан:** С. Одгэрэл
**Код:** B232270099
**Хичээл:** F.CSA313 - Программ хангамжийн чанарын баталгаа ба тест

## Орчин

java -version:

```
openjdk version "17.0.21.0.2" 2026-10-22 LTS
OpenJDK Runtime Environment Microsoft-15779939 (build 17.0.21.0.2+1-LTS)
OpenJDK 64-Bit Server VM Microsoft-15779939 (build 17.0.21.0.2+1-LTS, mixed mode, sharing)
```

mvn -version:

```
Apache Maven 3.9.16 (2bdd9fddda4b155ebf8000e807eb73fd829a51d5)
Maven home: C:\Users\Surface\Downloads\apache-maven-3.9.16-bin\apache-maven-3.9.16
Java version: 17.0.21.0.2, vendor: Microsoft, runtime: C:\Program Files\Microsoft\jdk-17.0.21.21-hotspot
Default locale: en_US, platform encoding: Cp1252
OS name: "windows 11", version: "10.0", arch: "amd64", family: "windows"
```

## Хэсэг A: Тестүүд

`results/mvn-test.txt`-ийн төгсгөл: **Tests run: 10, Failures: 0, Errors: 0, Skipped: 0**, BUILD SUCCESS.

10 тест = 7 энгийн тест (A1-A7) + A8-ийн 3 мөр (параметржүүлсэн тест).

### A8 дахь lenient() тайлбар

A8-ийн мөр бүр өөр нөхцөл тохируулдаг тул зарим мөрөнд зарим stub (жишээ нь courseExists) огт дуудагддаггүй. Mockito-гийн strict stubs горим ашиглагдаагүй stub-ийг UnnecessaryStubbingException алдаа болгодог тул lenient().when(...) хэрэглэж, мөр бүрд хэрэггүй stub-ийг зөвшөөрсөн.

## Хэсэг B: Мини мутаци

| № | Мутант (кодын өөрчлөлт) | Үр дүн | Унасан тестүүд |
|---|---|---|---|
| 1 | `if (!student.active()) ...` мөрийг комментлосон | Failures: 2, Errors: 0, BUILD FAILURE | A3 (register_inactiveStudent_returnsInactiveAndNeverTouchesEmail), A8-ийн 2-р мөр (register_errorClasses_returnExpectedCodeAndDoNotSave) |
| 2 | `repository.saveRegistration(...)` дуудалтыг комментлосон | Failures: 2, Errors: 0, BUILD FAILURE | A1 (register_activeStudentWithPrerequisites_returnsOkSavesAndSendsEmail), A5 (register_emailSenderThrows_stillReturnsOkAndSaves) |
| 3 | эхний if-ээс `\|\| courseId == null` устгасан | Failures: 1, Errors: 0, BUILD FAILURE | A6 (register_nullArguments_throwsIllegalArgumentAndTouchesNothing) |

Гурван мутант бүгд баригдсан. A6 хоёр аргументийн null-ийг тусад нь шалгадаг тул 3-р мутант эхний ажиллуулалтаас л баригдсан, тест засах шаардлагагүй байсан. Мутант бүрийн дараа код эх хэлбэрт нь сэргээгдсэн, эцсийн results/mvn-test.txt ногоон.

## Дүгнэлт

Mock нь дуудалтыг бүртгэж, verify-аар баталгаажуулдаг, бидний зааснаар хариу өгдөг объект бол гараар бичсэн fake нь санах ойн Map гэх мэт жинхэнэ логиктой хөнгөн хэрэгжүүлэлт юм. Энэ лабораторид StudentRepository болон EmailSender-т бодит хэрэгжүүлэлт байхгүй тул mock ашиглаж, өгөгдлийн сан, SMTP сервергүйгээр бизнес логикоо тусгаарлан тестэлсэн. Харин Student record, List зэрэг энгийн өгөгдлийн төрлүүдийг mock хийх ёсгүй, жинхэнийг нь шууд үүсгэх нь илүү энгийн бөгөөд найдвартай. Мөн тестлэж буй CourseRegistrationService классаа өөрийг нь mock хийвэл юу ч шалгахгүй болно. verify-г хэтрүүлбэл тест нь үр дүнгийн оронд хэрэгжүүлэлтийн дэлгэрэнгүйд (дуудалтын тоо, дараалал) зууралдаж, зөв refactor хийхэд ч унадаг эмзэг тест болно. Мутацийн хэсгээс харахад 1-р мутантад идэвхгүй оюутны шалгалтыг авмагц A3 нь ERROR_INACTIVE_STUDENT-ийн оронд ERROR_NO_COURSE авч, A8-ийн нэг мөр OK авч унасан. 2-р мутантад saveRegistration дуудалтыг авахад verify шалгасан A1, A5 унасан нь verify бол хадгалалтын дуудалтыг баталгаажуулдаг гол хэрэгсэл гэдгийг харуулсан. 3-р мутантад courseId-ийн