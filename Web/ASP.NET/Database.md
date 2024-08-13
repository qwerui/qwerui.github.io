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