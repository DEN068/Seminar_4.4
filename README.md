# Seminar_4.4
Урок 4. Зависимости в тестах

1) ***Почему использование тестовых заглушек может быть полезным при написании модульных тестов?**

1. Изолированность: Заглушки позволяют изолировать тестируемый компонент от его зависимостей. 
Это означает, что можно сосредоточиться на тестировании только одного компонента, не заботясь о правильной работе его зависимостей. Заглушки могут имитировать поведение зависимостей, что позволяет контролировать и моделировать различные сценарии для тестирования.

2. Контролируемые условия: Заглушки позволяют создавать предопределенные условия для тестовых сценариев. 
Можно указать, какие значения или поведение должны быть возвращены заглушкой в зависимости от того, какие параметры переданы и какие вызовы методов были сделаны. Это позволяет создать предсказуемые условия для проведения тестовых сценариев и проверки правильности работы тестируемого компонента.

3. Увеличение скорости выполнения тестов: Использование заглушек может помочь вам ускорить выполнение тестов, поскольку они избегает обращения к реальным сложным и медленным зависимостям, таким как база данных или внешние API. Вместо этого, заглушки объектов предоставляют заранее определенные результаты или поведение, которые значительно быстрее.

4. Тесирование различных сценариев: Заглушки позволяют вам тестировать различные сценарии, включая пограничные случаи и ошибки, которые могут быть трудными или невозможными воссоздать с реальными зависимостями. Заглушки могут возвращать предопределенные значения или бросать исключения, что позволяет вам проверить, как ваш компонент обрабатывает эти ситуации.

Тестовые заглушки помогают упростить создание и выполнение модульных тестов, улучшить их стабильность и избежать нежелательных взаимодействий с реальными зависимостями. Они способствуют созданию надежных и предсказуемых тестов, что помогает обнаружить и исправить ошибки в коде и улучшить качество программного обеспечения.


2) ***Какой тип тестовой заглушки следует использовать, если вам нужно проверить, что метод был вызван с определенными аргументами?**

Проверить, что метод был вызван с определенными аргументами, используют мок-объект или стуб с проверками (mock with verification) при использовании фреймворка тестирования, такого как Mockito.


3) ***Какой тип тестовой заглушки следует использовать, если вам просто нужно вернуть определенное значение или исключение в ответ на вызов метода?***

Нужно вернуть определенное значение или исключение в ответ на вызов метода без проверки вызова метода или его аргументов, то вам следует использовать тестовый стаб или заглушку с простой реализацией (stub with simple implementation).


4) ***Какой тип тестовой заглушки вы бы использовали для имитации  взаимодействия с внешним API или базой данных?***

Для имитации взаимодействия с внешним API или базой данных рекомендуется использовать мок-объект или фейковый объект.

Мок-объекты обычно используются для создания заглушек, которые имитируют поведение внешних зависимостей. 
С помощью мок-объектов вы можете настроить, какие результаты или исключения должны быть возвращены в ответ на вызовы методов. Это позволяет вам создавать предопределенные условия для тестовых сценариев. 



***У вас есть класс BookService, который использует интерфейс BookRepository для получения информации о книгах из базы данных. Ваша задача написать unit-тесты для BookService, используя Mockito для создания мок-объекта BookRepository.***

public interface BookRepository {
    List<Book> getAllBooks();
    Book getBookById(int id);
    void addBook(Book book);
    void updateBook(Book book);
    void deleteBook(int id);
}

public class Book {
    // Код класса Book
}

public class BookService {
    private BookRepository bookRepository;

    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public List<Book> getAllBooks() {
        return bookRepository.getAllBooks();
    }

    public Book getBookById(int id) {
        return bookRepository.getBookById(id);
    }

    public void addBook(Book book) {
        bookRepository.addBook(book);
    }

    public void updateBook(Book book) {
        bookRepository.updateBook(book);
    }

    public void deleteBook(int id) {
        bookRepository.deleteBook(id);
    }
}

public class BookServiceTest {
    private BookRepository bookRepository;
    private BookService bookService;

    @BeforeEach
    public void setUp() {
        bookRepository = Mockito.mock(BookRepository.class);
        bookService = new BookService(bookRepository);
    }

    @Test
    public void testGetAllBooks() {
        // Arrange
        List<Book> expectedBooks = Arrays.asList(
                new Book(1, "Book 1"),
                new Book(2, "Book 2")
        );
        Mockito.when(bookRepository.getAllBooks()).thenReturn(expectedBooks);

        // Act
        List<Book> actualBooks = bookService.getAllBooks();

        // Assert
        assertEquals(expectedBooks, actualBooks);
    }

    @Test
    public void testGetBookById() {
        // Arrange
        int bookId = 1;
        Book expectedBook = new Book(bookId, "Book 1");
        Mockito.when(bookRepository.getBookById(bookId)).thenReturn(expectedBook);

        // Act
        Book actualBook = bookService.getBookById(bookId);

        // Assert
        assertEquals(expectedBook, actualBook);
    }

    // Тесты для других методов класса BookService
