# Database

## .Net의 데이터베이스 처리 관련 클래스
ASP는 DB마다 다른 클래스를 사용한다. 아래의 클래스의 접두어로 DB이름이 붙으며, 아래 클래스 별로 같은 기능을 수행한다. 
- Connection : 데이터베이스 연결
- Command : 명령 실행
- DataReader : Select의 결과값
- DataSet : 메모리 상의 데이터베이스로 Select같은 결과 값 저장
- DataAdapter : 명령어 전달 및 실행 후 DataSet에 값 전달
- DataTable : DataSet 안의 메모리 상의 테이블
- DataView : DataSet 안의 메모리 상의 뷰

## 주요 작업
### 연결
```C#
SqlConnection con = new SqlConnection();
// Web.config에 설정된 값
con.ConnectionString = ConfigurationManager.ConnectcionStrings["ConnectionString"].ConnectionString;
con.Open();
// DB 작업
con.Close();
```

### 명령
```C#
SqlCommand cmd = new SqlCommand();
cmd.Connection = con; // 명령에 사용할 커넥션 지정
cmd.CommandText = @"sql문"; //sql또는 프로시저 문 지정
cmd.CommandType = CommandType.Text // CommandType의 형식, Text=SQL, StoredProcedure = 프로시저
cmd.ExecuteNonQuery(); // Select 이외의 명령, 영향 받은 행의 개수 반환
cmd.ExecuteReader(); // Select문 실행, 다중 값 반환
cmd.ExecuteScalar(); // Select문 실행, 단일 값 반환
```

### 결과 처리
```C#
SqlDataReader reader = cmd.ExecuteReader();
reader.FieldCount; // Select 문 실행결과에서 열의 개수
reader.HasRows; // Select 문 실행 후 반환되는 레코드 존재 여부
while(reader.Read())
{
    reader["컬럼명"];
    reader.GetString(1); // 매개변수로 인덱스를 받는다.
}
reader.Close();
```

### DataAdapter & DataSet
메모리에 테이블 등을 생성해 데이터를 조작하는 클래스   

**DataAdapter와 DataReader의 차이**   
- DataReader: 연결 지향적이고, 단방향, 읽기 전용으로 데이터를 순차적으로 읽어야 할 때 사용. 성능이 중요하고 데이터를 한 번에 한 행씩 처리해야 할 때 적합.
- DataAdapter: 비연결 지향적이고, 데이터를 로컬로 가져와 조작 및 캐싱해야 할 때 사용. 오프라인 데이터 처리 및 데이터베이스에 대한 대량의 변경 사항을 반영할 때 적합.

```C#
SqlDataAdapter adapter = new SqlDataAdapter();
adapter.SelectCommand = cmd;
DataSet ds = new DataSet();
adapter.Fill(ds, "테이블이름");
foreach (DataRow row in dataSet.Tables["테이블이름"].Rows)
    {
        Console.WriteLine(row["컬럼이름"]);
    }
```

## Entity Framework
.Net의 ORM, Java의 JPA와 비슷한듯 하다. 실제로 DB 연결을 담당하는 클래스는 DbContext이며 일반적으로는 1개의 클래스만 사용한다. 아래의 경우에는 여러개를 사용할 수 있다[(출처)](https://www.milanjovanovic.tech/blog/using-multiple-ef-core-dbcontext-in-single-application#creating-multiple-dbcontexts-in-a-single-application). 여러개를 사용한다면 스키마 단위로 나누는 것이 좋아보인다.
- 여러 데이터베이스에 접속해야하는 경우
- 도메인 모델이 복잡해 명확한 분리가 필요한 경우
- 모듈러 모놀리식을 구축할 경우
- 읽기 전용 복제본에 접근할 경우(NoTracking으로 성능 향상 가능)

### DB에 변경 내용 반영
```sh
dotnet tool install --global dotnet-ef # dotnet ef 설치
dotnet ef migrations add init # 마이그레이션 추가, 코드 변경 이후 사용, init은 이름이니 변경가능
dotnet ef database update # 데이터베이스 생성
```

### Model
```c#
// 대체로 선언방법과 관계형성은 JPA와 비슷하다.
namespace CrosswordBackend.Models
{
    public class Crossword
    {
        [Key] //기본키 지정
        public int Id { get; set; }
        public string Info { get; set; }
        public DateTime Date { get; set; }

        // 외래키 다
        public virtual IList<Answer> Answers { get; set; } = new List<Answer>();
        public virtual IList<Record> Records { get; set; } = new List<Record>();
    }
}

namespace CrosswordBackend.Models
{
    [PrimaryKey(nameof(CrosswordId), nameof(BlockNumber), nameof(IsVertical))] //복합키 지정
    public class Answer
    {
        // JPA와 다르게 Id 필드를 선언해야한다.
        public int CrosswordId { get; set; }
        public int WordId { get; set; }
        public int BlockNumber { get; set; }
        public bool IsVertical { get; set; }

        // 외래키 일
        public virtual Word Word { get; set; }
        public virtual Crossword Crossword { get; set; }
    }
}

// DBContext = DB 세션, 사용 방법은 2가지가 있다
// 1. WebApplicationBuilder에 등록후 DI로 사용
// 2. using문에서 직접 생성해서 사용
namespace CrosswordBackend.Config
{
    public class ApplicationDbContext : DbContext
    {
        // 필드명 기준으로 테이블 명이 결정된다.
        public DbSet<Word> Word {  get; set; }
        public DbSet<Crossword> Crossword { get; set; }
        public DbSet<Meaning> Meaning { get; set; }
        public DbSet<Answer> Answer { get; set; }
        public DbSet<Record> Record { get; set; }

        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options) { }
    }
}

```

### Query
```c#
// 대체로 Java의 Querydsl과 비슷하다.
using (var context = new ApplicationDbContext())
{
    // DTO 사용
    var products = context.Products
                          .Where(p => p.Price > 100)
                          .Select(p => new ProductDto
                          {
                              Name = p.Name,
                              Price = p.Price
                          })
                          .ToList();

    // Join
    var products = from p in context.Products
                   join c in context.Categories on p.CategoryId equals c.CategoryId
                   select new ProductDto
                   {
                       Name = p.Name,
                       Price = p.Price,
                       CategoryName = c.Name
                   };

    var result = products.ToList();

    // Create
    await context.Products.AddAsync(new ProductModel());

    // 기본적으로 조회 후 변경 및 삭제를 수행한다. JPA와 비슷
    // Update
    context.Products.Update(result);
    // Delete
    context.Products.Remove(result);
    // DB에 반영, 암시적으로 트랜잭션을 수행한다.
    await context.SaveChangesAsync();

}
```