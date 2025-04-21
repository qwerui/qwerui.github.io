# Batch

## 주요 개념
- Job : 하나의 배치 작업 단위. 여러 Step을 포함할 수 있음
- Step : Job을 구성하는 단위 작업. Tasklet 또는 Chunk 기반
- Job 하나 당 Step이 순차적으로 실행, Chunk 기반 Stop은 Reader -> Processor -> Writer 순으로 진행

### Job 관련 클래스

클래스명 | 설명
Job | 하나의 배치 작업 단위. 여러 Step을 포함할 수 있음
Step | Job을 구성하는 단위 작업. Tasklet 또는 Chunk 기반
JobLauncher | Job을 실행시키는 컴포넌트
JobBuilder | Job을 구성하는 빌더
JobBuilderFactory | JobBuilder를 생성해주는 팩토리
JobExecution | Job이 실제 실행될 때 생성되는 실행 정보 (상태, 시간 등 포함)
JobParameters | Job 실행 시 전달되는 파라미터
JobInstance | Job 이름 + JobParameters 조합으로 구분되는 고유 인스턴스
JobRepository | Job/Step 실행 결과, 상태, 이력 등을 DB에 저장 및 관리

### Step 관련 클래스

클래스명 | 설명
StepBuilder | Step을 구성하는 빌더
StepBuilderFactory | StepBuilder를 생성해주는 팩토리
Tasklet | Step 내에서 단일 작업을 수행할 수 있는 인터페이스
ChunkOrientedTasklet | Chunk 기반 처리를 위한 내부 구현체
StepExecution | Step의 실행 정보를 담고 있음 (건수, 상태 등)

### Chunk 관련 클래스

클래스명 | 설명
ItemReader<T> | 데이터를 한 건씩 읽는 역할
ItemProcessor<I, O> | 읽은 데이터를 가공/필터링
ItemWriter<T> | 가공된 데이터를 저장 또는 출력
FlatFileItemReader, JdbcCursorItemReader, JpaPagingItemReader | 자주 쓰이는 Reader 구현체
JdbcBatchItemWriter, FlatFileItemWriter | 자주 쓰이는 Writer 구현체

## 설정 예시

```java

@Configuration
public class MemberBatch {
    @Bean
    public JdbcCursorItemReader<Member> itemReader(DataSource dataSource) {
        JdbcCursorItemReader<Member> reader = new JdbcCursorItemReader<>();
        reader.setDataSource(dataSource);
        reader.setSql("SELECT id, name FROM member");
        reader.setRowMapper((rs, rowNum) -> new Member(rs.getLong("id"), rs.getString("name")));
        return reader;
    }

    @Bean
    public ItemProcessor<Member, Member> itemProcessor() {
        return member -> {
            member.setName(member.getName().toUpperCase());  // 이름을 대문자로 변환
            return member;
        };
    }

    @Bean
    public JdbcBatchItemWriter<Member> itemWriter(DataSource dataSource) {
        JdbcBatchItemWriter<Member> writer = new JdbcBatchItemWriter<>();
        writer.setDataSource(dataSource);
        writer.setSql("INSERT INTO member_processed (id, name) VALUES (:id, :name)");
        writer.setItemSqlParameterSourceProvider(new BeanPropertyItemSqlParameterSourceProvider<>());
        return writer;
    }

    @Bean
    public Job memberJob(JobRepository jobRepository, Step step) {
        return new JobBuilder("memberJob", jobRepository)
                .start(step)
                .build();
    }

    @Bean
    public Step step(JobRepository jobRepository, PlatformTransactionManager transactionManager,
                     ItemReader<Member> reader, ItemProcessor<Member, Member> processor, ItemWriter<Member> writer) {
        return new StepBuilder("step", jobRepository)
                .<Member, Member>chunk(10, transactionManager)
                .reader(reader)
                .processor(processor)
                .writer(writer)
                .build();
    }
}

```

## 실행 예시

```java

@Component
@RequiredArgsConstructor
public class MemberRunner implements CommandLineRunner {

    private final JobLauncher jobLauncher;
    private final Job memberJob;

    @Override
    public void run(String... args) throws Exception {
        JobParameters params = new JobParametersBuilder()
                .addLong("time", System.currentTimeMillis())  // JobInstance 구분용
                .toJobParameters();

        jobLauncher.run(memberJob, params);
    }
}

```