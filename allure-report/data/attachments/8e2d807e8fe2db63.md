# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: dataTable/dataTable-structure.spec.js >> QA Playground - Data Table - Structure Validation >> Validate Genre filter reduces visible rows to the selected genre only
- Location: tests/dataTable/dataTable-structure.spec.js:138:5

# Error details

```
Error: Genre Mismatch at Row 1 |
                Expected: Science Fiction |
                Actual: Technology

expect(received).toBe(expected) // Object.is equality

Expected: "Science Fiction"
Received: "Technology"
```

```
Error: expect(locator).toHaveValue(expected) failed

Locator:  locator('[data-testid="genre-filter"]')
Expected: "Science Fiction"
Received: "All"

Call log:
  - Expect "toHaveValue" locator('[data-testid="genre-filter"]') with timeout 5000ms
  - waiting for locator('[data-testid="genre-filter"]')
    6 × locator resolved to <select id="genre-filter-select" data-testid="genre-filter" aria-label="Filter by genre" class="data-table-module__WJUPia__filterSelect">…</select>
      - unexpected value "All"
  - Test ended.

```

```yaml
- combobox "Filter by genre":
  - option "All Genres" [selected]
  - option "Technology"
  - option "Fantasy"
  - option "Science Fiction"
  - option "Dystopian"
  - option "Fiction"
  - option "Non-Fiction"
```

# Test source

```ts
  49  |         // Get Total Row
  50  |         const actualRow = 
  51  |             await dataTablesPage.getTableRowCount();
  52  |         // Validate Total Row
  53  |         expect(
  54  |             actualRow,
  55  |             `Total Record Count Mismatch |
  56  |             Expected to contain: "${DataTablesData.totalRow}" | Actual: "${actualRow}"`
  57  |         ).toContain(DataTablesData.totalRow);
  58  |     });
  59  | 
  60  |     test('Validate Genre Options', 
  61  |     {
  62  |         tag: ['@regression', '@positive']
  63  |     },
  64  |     async ({ dataTablesPage }) => {
  65  |         // Get Genre Options
  66  |         const genreOptions = 
  67  |             await dataTablesPage.getGenreOptions();
  68  |         // Validate Genre Options
  69  |         expect(
  70  |             genreOptions,
  71  |             `Genre Options Mismatch |
  72  |             Expected: ${DataTablesData.genreOptions} | Actual: ${genreOptions}`
  73  |         ).toEqual(DataTablesData.genreOptions);
  74  |     });
  75  | 
  76  |     test('Validate Default Book Names', 
  77  |     {
  78  |         tag: ['@regression', '@positive']
  79  |     },
  80  |     async ({ dataTablesPage }) => {
  81  |         // Get Book Name Column values
  82  |         const actualBookNames = 
  83  |             await dataTablesPage.getColumnRows(
  84  |                 'bookName'
  85  |             );
  86  |         // Validate no empty values
  87  |         actualBookNames.forEach((bookName, index) => {
  88  |             expect(
  89  |                 bookName,
  90  |                 `Book Name is empty at Row ${index + 1}`
  91  |             ).not.toBe('');
  92  |         });
  93  |         // Validate Book Name Column values
  94  |         expect(
  95  |             actualBookNames,
  96  |             `Book Name Column values Mismatch |
  97  |             Expected: ${DataTablesData.bookNames} | Actual: ${actualBookNames}`
  98  |         ).toEqual(DataTablesData.bookNames);
  99  |     });
  100 | 
  101 |     // test('Validate Default Exact Book Name Column Values', 
  102 |     // {
  103 |     //     tag: ['@regression', '@positive']
  104 |     // },
  105 |     // async ({ dataTablesPage }) => {
  106 |     //     // Get Book Name Column values
  107 |     //     const actualBookNames = 
  108 |     //         await dataTablesPage.getBookNameRows();
  109 |     //     // Validate exact Book Name Column values
  110 |     //     for (let i = 0; i < DataTablesData.bookNames.length; i++) {
  111 |     //         expect(
  112 |     //             actualBookNames[i],
  113 |     //             `Book Name Column values Mismatch at row ${i + 1} |
  114 |     //             Expected: ${DataTablesData.bookNames[i]} | Actual: ${actualBookNames[i]}`
  115 |     //         ).toEqual(DataTablesData.bookNames[i]);
  116 |     //     }
  117 |     // });
  118 | 
  119 |     test('Validate All values in the Book ISBN column start with ISBN-', 
  120 |     {
  121 |         tag: ['@regression', '@positive']
  122 |     },
  123 |     async ({ dataTablesPage }) => {
  124 |         // Get ISBN column values
  125 |         const isbnValues = 
  126 |             await dataTablesPage.getColumnRows(
  127 |                 'bookIsbn'
  128 |             );
  129 |         // Validate ISBN column values
  130 |         isbnValues.forEach(isbn => {
  131 |             expect(
  132 |                 isbn.trim(),
  133 |                 `Invalid ISBN: ${isbn}`
  134 |             ).toMatch(/^ISBN-/);
  135 |         })
  136 |     });
  137 | 
  138 |     test('Validate Genre filter reduces visible rows to the selected genre only', 
  139 |     {
  140 |         tag: ['@regression', '@positive']
  141 |     },
  142 |     async ({ dataTablesPage }) => {
  143 |         // Select Genre
  144 |         await dataTablesPage.selectGenre(
  145 |             'filter',
  146 |             DataTablesData.existingBook.genre
  147 |         );
  148 |         const selectedGenre = dataTablesPage.getGenreDropdown();
> 149 |         expect(selectedGenre).toHaveValue(
      |                               ^ Error: expect(locator).toHaveValue(expected) failed
  150 |             DataTablesData.existingBook.genre
  151 |         );
  152 |         // Validate Genre column
  153 |         const actualBookGenres =
  154 |             await dataTablesPage.getColumnRows(
  155 |                 'bookGenre'
  156 |             );
  157 |         DataTablesAssertions.validateGenres(
  158 |             actualBookGenres,
  159 |             DataTablesData.existingBook.genre
  160 |         );
  161 |     });
  162 |     
  163 | });
```