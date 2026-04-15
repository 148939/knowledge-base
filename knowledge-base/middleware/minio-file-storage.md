# 检索索引：本文档收录【MinIO】【文件存储】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

MinIO 是一个基于 Apache License v2.0 开源协议的对象存储服务。它兼容 Amazon S3 云存储服务接口，非常适合于存储大容量非结构化的数据，例如图片、视频、日志文件、备份数据和容器/虚拟机镜像等。MinIO 是一个非常轻量级的服务，可以很简单地和其他应用程序集成，类似于 NodeJS、Redis 或 MySQL。

## 2. 核心知识点（结构化层级）

### 2.1 基础概念
1. 对象存储
2. Bucket（存储桶）
3. Object（对象）
4. 前缀
5. 版本控制
6. 生命周期管理
7. 存储策略

### 2.2 安装部署
1. Docker 部署
2. 二进制部署
3. Kubernetes 部署
4. 分布式部署
5. 单机部署

### 2.3 核心功能
1. 存储桶管理
   - 创建存储桶
   - 删除存储桶
   - 列出存储桶
   - 存储桶策略
2. 对象管理
   - 上传对象
   - 下载对象
   - 删除对象
   - 复制对象
3. 分片上传
4. 预签名 URL
5. 事件通知
6. 生命周期管理

### 2.4 安全设置
1. 访问密钥
2. 存储桶策略
3. IAM 策略
4. 加密
5. 访问日志

### 2.5 SDK 使用
1. Java SDK
2. Python SDK
3. JavaScript SDK
4. Go SDK
5. Spring Boot 集成

## 3. 官方标准语法 / API

### 安装部署
```bash
# Docker 部署
docker run -d \
  -p 9000:9000 \
  -p 9001:9001 \
  --name minio \
  -v /mnt/data:/data \
  -e "MINIO_ROOT_USER=admin" \
  -e "MINIO_ROOT_PASSWORD=admin123" \
  minio/minio server /data --console-address ":9001"

# 二进制部署（Linux）
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
MINIO_ROOT_USER=admin MINIO_ROOT_PASSWORD=admin123 ./minio server /data --console-address ":9001"

# 分布式部署
minio server http://node{1...4}/mnt/data{1...2}

# Kubernetes 部署（使用 Helm）
helm repo add minio https://charts.min.io/
helm install minio minio/minio \
  --set rootUser=admin \
  --set rootPassword=admin123 \
  --set persistence.enabled=true \
  --set persistence.size=10Gi
```

### mc 命令行工具
```bash
# 下载 mc 客户端
wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc

# 配置别名
mc alias set myminio http://localhost:9000 admin admin123

# 列出存储桶
mc ls myminio

# 创建存储桶
mc mb myminio/mybucket

# 删除存储桶
mc rb myminio/mybucket

# 上传文件
mc cp localfile.txt myminio/mybucket/

# 下载文件
mc cp myminio/mybucket/remote.txt local.txt

# 删除对象
mc rm myminio/mybucket/object.txt

# 列出对象
mc ls myminio/mybucket

# 设置存储桶策略
mc anonymous set public myminio/mybucket
mc anonymous set private myminio/mybucket
mc anonymous set download myminio/mybucket

# 查看存储桶策略
mc anonymous get myminio/mybucket

# 生成预签名 URL
mc share download myminio/mybucket/object.txt
mc share upload myminio/mybucket/

# 监控
mc admin info myminio
mc admin heal myminio
```

### Java SDK 使用
```xml
<!-- pom.xml -->
<dependency>
    <groupId>io.minio</groupId>
    <artifactId>minio</artifactId>
    <version>8.5.7</version>
</dependency>
```

```java
import io.minio.*;
import io.minio.messages.Bucket;
import io.minio.messages.Item;
import java.io.File;
import java.io.InputStream;
import java.util.List;

// MinIO 配置
MinioClient minioClient = MinioClient.builder()
        .endpoint("http://localhost:9000")
        .credentials("admin", "admin123")
        .build();

// 检查存储桶是否存在
boolean found = minioClient.bucketExists(BucketExistsArgs.builder()
        .bucket("mybucket")
        .build());

// 创建存储桶
if (!found) {
    minioClient.makeBucket(MakeBucketArgs.builder()
            .bucket("mybucket")
            .build());
}

// 列出所有存储桶
List<Bucket> buckets = minioClient.listBuckets();
for (Bucket bucket : buckets) {
    System.out.println(bucket.name() + ", " + bucket.creationDate());
}

// 上传文件
minioClient.uploadObject(UploadObjectArgs.builder()
        .bucket("mybucket")
        .object("myobject.txt")
        .filename("/path/to/localfile.txt")
        .build());

// 上传 InputStream
InputStream inputStream = new FileInputStream("/path/to/localfile.txt");
minioClient.putObject(PutObjectArgs.builder()
        .bucket("mybucket")
        .object("myobject.txt")
        .stream(inputStream, -1, 10485760)
        .contentType("text/plain")
        .build());

// 下载文件
minioClient.downloadObject(DownloadObjectArgs.builder()
        .bucket("mybucket")
        .object("myobject.txt")
        .filename("/path/to/download.txt")
        .build());

// 获取对象 InputStream
InputStream stream = minioClient.getObject(GetObjectArgs.builder()
        .bucket("mybucket")
        .object("myobject.txt")
        .build());

// 删除对象
minioClient.removeObject(RemoveObjectArgs.builder()
        .bucket("mybucket")
        .object("myobject.txt")
        .build());

// 批量删除对象
List<DeleteObject> objects = new ArrayList<>();
objects.add(new DeleteObject("object1.txt"));
objects.add(new DeleteObject("object2.txt"));
minioClient.removeObjects(RemoveObjectsArgs.builder()
        .bucket("mybucket")
        .objects(objects)
        .build());

// 列出对象
Iterable<Result<Item>> results = minioClient.listObjects(ListObjectsArgs.builder()
        .bucket("mybucket")
        .build());
for (Result<Item> result : results) {
    Item item = result.get();
    System.out.println(item.objectName() + ", " + item.size());
}

// 检查对象是否存在
try {
    minioClient.statObject(StatObjectArgs.builder()
            .bucket("mybucket")
            .object("myobject.txt")
            .build());
} catch (ErrorResponseException e) {
    if (e.errorResponse().code().equals("NoSuchKey")) {
        System.out.println("Object does not exist");
    }
}

// 复制对象
minioClient.copyObject(CopyObjectArgs.builder()
        .bucket("mybucket")
        .object("copy.txt")
        .source(CopySource.builder()
                .bucket("mybucket")
                .object("source.txt")
                .build())
        .build());

// 生成预签名 URL（下载）
String url = minioClient.getPresignedObjectUrl(GetPresignedObjectUrlArgs.builder()
        .bucket("mybucket")
        .object("myobject.txt")
        .method(Method.GET)
        .expiry(1, TimeUnit.HOURS)
        .build());

// 生成预签名 URL（上传）
Map<String, String> params = new HashMap<>();
String postPolicyFormUrl = minioClient.getPresignedObjectUrl(GetPresignedObjectUrlArgs.builder()
        .bucket("mybucket")
        .object("myobject.txt")
        .method(Method.POST)
        .expiry(1, TimeUnit.HOURS)
        .extraQueryParams(params)
        .build());

// 设置存储桶策略
String policy = "{\n" +
        "  \"Version\": \"2012-10-17\",\n" +
        "  \"Statement\": [\n" +
        "    {\n" +
        "      \"Effect\": \"Allow\",\n" +
        "      \"Principal\": {\"AWS\": \"*\"},\n" +
        "      \"Action\": [\"s3:GetObject\"],\n" +
        "      \"Resource\": [\"arn:aws:s3:::mybucket/*\"]\n" +
        "    }\n" +
        "  ]\n" +
        "}";
minioClient.setBucketPolicy(SetBucketPolicyArgs.builder()
        .bucket("mybucket")
        .config(policy)
        .build());

// 获取存储桶策略
String bucketPolicy = minioClient.getBucketPolicy(GetBucketPolicyArgs.builder()
        .bucket("mybucket")
        .build());
```

### Spring Boot 集成
```xml
<!-- pom.xml -->
<dependency>
    <groupId>io.minio</groupId>
    <artifactId>minio</artifactId>
    <version>8.5.7</version>
</dependency>
```

```yaml
# application.yml
minio:
  endpoint: http://localhost:9000
  accessKey: admin
  secretKey: admin123
  bucketName: mybucket
```

```java
@Configuration
public class MinioConfig {
    
    @Value("${minio.endpoint}")
    private String endpoint;
    
    @Value("${minio.accessKey}")
    private String accessKey;
    
    @Value("${minio.secretKey}")
    private String secretKey;
    
    @Bean
    public MinioClient minioClient() {
        return MinioClient.builder()
                .endpoint(endpoint)
                .credentials(accessKey, secretKey)
                .build();
    }
}

@Service
public class MinioService {
    
    @Autowired
    private MinioClient minioClient;
    
    @Value("${minio.bucketName}")
    private String bucketName;
    
    @PostConstruct
    public void init() {
        try {
            boolean found = minioClient.bucketExists(BucketExistsArgs.builder()
                    .bucket(bucketName)
                    .build());
            if (!found) {
                minioClient.makeBucket(MakeBucketArgs.builder()
                        .bucket(bucketName)
                        .build());
            }
        } catch (Exception e) {
            throw new RuntimeException("初始化 MinIO 失败", e);
        }
    }
    
    public String uploadFile(MultipartFile file) {
        String originalFilename = file.getOriginalFilename();
        String filename = UUID.randomUUID().toString() + "." + 
                getExtension(originalFilename);
        
        try (InputStream inputStream = file.getInputStream()) {
            minioClient.putObject(PutObjectArgs.builder()
                    .bucket(bucketName)
                    .object(filename)
                    .stream(inputStream, file.getSize(), -1)
                    .contentType(file.getContentType())
                    .build());
            
            return filename;
        } catch (Exception e) {
            throw new RuntimeException("上传文件失败", e);
        }
    }
    
    public InputStream downloadFile(String filename) {
        try {
            return minioClient.getObject(GetObjectArgs.builder()
                    .bucket(bucketName)
                    .object(filename)
                    .build());
        } catch (Exception e) {
            throw new RuntimeException("下载文件失败", e);
        }
    }
    
    public void deleteFile(String filename) {
        try {
            minioClient.removeObject(RemoveObjectArgs.builder()
                    .bucket(bucketName)
                    .object(filename)
                    .build());
        } catch (Exception e) {
            throw new RuntimeException("删除文件失败", e);
        }
    }
    
    public String getPresignedUrl(String filename, int expiryMinutes) {
        try {
            return minioClient.getPresignedObjectUrl(GetPresignedObjectUrlArgs.builder()
                    .bucket(bucketName)
                    .object(filename)
                    .method(Method.GET)
                    .expiry(expiryMinutes, TimeUnit.MINUTES)
                    .build());
        } catch (Exception e) {
            throw new RuntimeException("生成预签名 URL 失败", e);
        }
    }
    
    public List<String> listFiles() {
        List<String> filenames = new ArrayList<>();
        try {
            Iterable<Result<Item>> results = minioClient.listObjects(ListObjectsArgs.builder()
                    .bucket(bucketName)
                    .build());
            for (Result<Item> result : results) {
                Item item = result.get();
                filenames.add(item.objectName());
            }
        } catch (Exception e) {
            throw new RuntimeException("列出文件失败", e);
        }
        return filenames;
    }
    
    private String getExtension(String filename) {
        if (filename == null || !filename.contains(".")) {
            return "";
        }
        return filename.substring(filename.lastIndexOf(".") + 1);
    }
}

@RestController
@RequestMapping("/files")
public class FileController {
    
    @Autowired
    private MinioService minioService;
    
    @PostMapping("/upload")
    public Result<String> upload(@RequestParam("file") MultipartFile file) {
        String filename = minioService.uploadFile(file);
        return Result.success(filename);
    }
    
    @GetMapping("/download/{filename}")
    public void download(@PathVariable String filename, HttpServletResponse response) {
        try (InputStream inputStream = minioService.downloadFile(filename);
             OutputStream outputStream = response.getOutputStream()) {
            
            response.setContentType("application/octet-stream");
            response.setHeader("Content-Disposition", 
                    "attachment; filename=\"" + URLEncoder.encode(filename, "UTF-8") + "\"");
            
            byte[] buffer = new byte[4096];
            int bytesRead;
            while ((bytesRead = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, bytesRead);
            }
        } catch (Exception e) {
            throw new RuntimeException("下载文件失败", e);
        }
    }
    
    @DeleteMapping("/{filename}")
    public Result<Void> delete(@PathVariable String filename) {
        minioService.deleteFile(filename);
        return Result.success();
    }
    
    @GetMapping("/url/{filename}")
    public Result<String> getUrl(@PathVariable String filename) {
        String url = minioService.getPresignedUrl(filename, 60);
        return Result.success(url);
    }
    
    @GetMapping
    public Result<List<String>> list() {
        List<String> files = minioService.listFiles();
        return Result.success(files);
    }
}
```

### Python SDK 使用
```python
from minio import Minio
from minio.error import S3Error
import io

# 创建 MinIO 客户端
client = Minio(
    "localhost:9000",
    access_key="admin",
    secret_key="admin123",
    secure=False
)

# 检查存储桶是否存在
found = client.bucket_exists("mybucket")
if not found:
    client.make_bucket("mybucket")

# 列出存储桶
buckets = client.list_buckets()
for bucket in buckets:
    print(bucket.name, bucket.creation_date)

# 上传文件
client.fput_object(
    "mybucket",
    "myobject.txt",
    "/path/to/localfile.txt",
)

# 上传文件对象
data = io.BytesIO(b"hello world")
client.put_object(
    "mybucket",
    "myobject.txt",
    data,
    len(data.getvalue()),
)

# 下载文件
client.fget_object(
    "mybucket",
    "myobject.txt",
    "/path/to/download.txt",
)

# 获取文件对象
response = client.get_object("mybucket", "myobject.txt")
data = response.read()

# 删除对象
client.remove_object("mybucket", "myobject.txt")

# 批量删除
objects_to_delete = ["object1.txt", "object2.txt"]
for obj in objects_to_delete:
    client.remove_object("mybucket", obj)

# 列出对象
objects = client.list_objects("mybucket")
for obj in objects:
    print(obj.object_name, obj.size)

# 获取对象信息
stat = client.stat_object("mybucket", "myobject.txt")
print(stat.size, stat.content_type)

# 复制对象
client.copy_object(
    "mybucket",
    "copy.txt",
    CopySource("mybucket", "source.txt"),
)

# 生成预签名 URL
url = client.presigned_get_object(
    "mybucket",
    "myobject.txt",
    expires=3600,
)

url = client.presigned_put_object(
    "mybucket",
    "myobject.txt",
    expires=3600,
)

# 设置存储桶策略
policy = {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {"AWS": "*"},
            "Action": ["s3:GetObject"],
            "Resource": ["arn:aws:s3:::mybucket/*"],
        },
    ],
}
client.set_bucket_policy("mybucket", json.dumps(policy))

# 获取存储桶策略
policy = client.get_bucket_policy("mybucket")
```

## 4. 官方示例代码（可运行）

### 文件上传下载完整示例
```java
@SpringBootApplication
public class MinioDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(MinioDemoApplication.class, args);
    }
}

@Data
@Configuration
@ConfigurationProperties(prefix = "minio")
public class MinioProperties {
    private String endpoint;
    private String accessKey;
    private String secretKey;
    private String bucketName;
}

@Configuration
@RequiredArgsConstructor
public class MinioConfig {
    
    private final MinioProperties minioProperties;
    
    @Bean
    public MinioClient minioClient() {
        return MinioClient.builder()
                .endpoint(minioProperties.getEndpoint())
                .credentials(minioProperties.getAccessKey(), minioProperties.getSecretKey())
                .build();
    }
}

@Service
@RequiredArgsConstructor
public class FileStorageService {
    
    private final MinioClient minioClient;
    private final MinioProperties minioProperties;
    
    private static final Logger log = LoggerFactory.getLogger(FileStorageService.class);
    
    @PostConstruct
    public void init() {
        createBucketIfNotExists();
        setBucketPolicy();
    }
    
    private void createBucketIfNotExists() {
        try {
            boolean exists = minioClient.bucketExists(BucketExistsArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .build());
            if (!exists) {
                minioClient.makeBucket(MakeBucketArgs.builder()
                        .bucket(minioProperties.getBucketName())
                        .build());
                log.info("Bucket {} created", minioProperties.getBucketName());
            }
        } catch (Exception e) {
            throw new RuntimeException("Failed to initialize bucket", e);
        }
    }
    
    private void setBucketPolicy() {
        try {
            String policy = "{\n" +
                    "  \"Version\": \"2012-10-17\",\n" +
                    "  \"Statement\": [\n" +
                    "    {\n" +
                    "      \"Effect\": \"Allow\",\n" +
                    "      \"Principal\": {\"AWS\": \"*\"},\n" +
                    "      \"Action\": [\"s3:GetObject\"],\n" +
                    "      \"Resource\": [\"arn:aws:s3:::" + minioProperties.getBucketName() + "/*\"]\n" +
                    "    }\n" +
                    "  ]\n" +
                    "}";
            
            minioClient.setBucketPolicy(SetBucketPolicyArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .config(policy)
                    .build());
        } catch (Exception e) {
            log.warn("Failed to set bucket policy", e);
        }
    }
    
    public String uploadFile(MultipartFile file) {
        String originalFilename = file.getOriginalFilename();
        String extension = getExtension(originalFilename);
        String filename = generateFilename(extension);
        String contentType = file.getContentType();
        
        try (InputStream inputStream = file.getInputStream()) {
            minioClient.putObject(PutObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .stream(inputStream, file.getSize(), -1)
                    .contentType(contentType)
                    .build());
            
            log.info("File uploaded: {}", filename);
            return filename;
        } catch (Exception e) {
            log.error("Failed to upload file", e);
            throw new RuntimeException("文件上传失败", e);
        }
    }
    
    public String uploadFileWithPath(MultipartFile file, String path) {
        String originalFilename = file.getOriginalFilename();
        String extension = getExtension(originalFilename);
        String filename = generateFilename(extension);
        String objectName = path.endsWith("/") ? path + filename : path + "/" + filename;
        String contentType = file.getContentType();
        
        try (InputStream inputStream = file.getInputStream()) {
            minioClient.putObject(PutObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(objectName)
                    .stream(inputStream, file.getSize(), -1)
                    .contentType(contentType)
                    .build());
            
            log.info("File uploaded: {}", objectName);
            return objectName;
        } catch (Exception e) {
            log.error("Failed to upload file", e);
            throw new RuntimeException("文件上传失败", e);
        }
    }
    
    public List<String> batchUploadFiles(List<MultipartFile> files) {
        List<String> filenames = new ArrayList<>();
        for (MultipartFile file : files) {
            filenames.add(uploadFile(file));
        }
        return filenames;
    }
    
    public void downloadFile(String filename, HttpServletResponse response) {
        try (InputStream inputStream = minioClient.getObject(GetObjectArgs.builder()
                        .bucket(minioProperties.getBucketName())
                        .object(filename)
                        .build());
             OutputStream outputStream = response.getOutputStream()) {
            
            StatObjectResponse stat = minioClient.statObject(StatObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .build());
            
            response.setContentType(stat.contentType());
            response.setContentLengthLong(stat.size());
            response.setHeader("Content-Disposition", 
                    "attachment; filename=\"" + URLEncoder.encode(filename, StandardCharsets.UTF_8) + "\"");
            
            byte[] buffer = new byte[8192];
            int bytesRead;
            while ((bytesRead = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, bytesRead);
            }
            
            log.info("File downloaded: {}", filename);
        } catch (Exception e) {
            log.error("Failed to download file", e);
            throw new RuntimeException("文件下载失败", e);
        }
    }
    
    public InputStream getFileInputStream(String filename) {
        try {
            return minioClient.getObject(GetObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .build());
        } catch (Exception e) {
            log.error("Failed to get file input stream", e);
            throw new RuntimeException("获取文件失败", e);
        }
    }
    
    public void deleteFile(String filename) {
        try {
            minioClient.removeObject(RemoveObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .build());
            log.info("File deleted: {}", filename);
        } catch (Exception e) {
            log.error("Failed to delete file", e);
            throw new RuntimeException("文件删除失败", e);
        }
    }
    
    public void batchDeleteFiles(List<String> filenames) {
        List<DeleteObject> deleteObjects = filenames.stream()
                .map(DeleteObject::new)
                .collect(Collectors.toList());
        
        try {
            minioClient.removeObjects(RemoveObjectsArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .objects(deleteObjects)
                    .build());
            log.info("Files deleted: {}", filenames);
        } catch (Exception e) {
            log.error("Failed to delete files", e);
            throw new RuntimeException("文件批量删除失败", e);
        }
    }
    
    public boolean fileExists(String filename) {
        try {
            minioClient.statObject(StatObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .build());
            return true;
        } catch (ErrorResponseException e) {
            if ("NoSuchKey".equals(e.errorResponse().code())) {
                return false;
            }
            throw new RuntimeException("检查文件失败", e);
        } catch (Exception e) {
            throw new RuntimeException("检查文件失败", e);
        }
    }
    
    public String getFileUrl(String filename) {
        return minioProperties.getEndpoint() + "/" + 
                minioProperties.getBucketName() + "/" + filename;
    }
    
    public String getPresignedDownloadUrl(String filename, int expiryMinutes) {
        try {
            return minioClient.getPresignedObjectUrl(GetPresignedObjectUrlArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .method(Method.GET)
                    .expiry(expiryMinutes, TimeUnit.MINUTES)
                    .build());
        } catch (Exception e) {
            log.error("Failed to generate presigned download url", e);
            throw new RuntimeException("生成下载链接失败", e);
        }
    }
    
    public Map<String, String> getPresignedUploadUrl(String filename, int expiryMinutes) {
        try {
            Map<String, String> formData = new HashMap<>();
            String url = minioClient.getPresignedObjectUrl(GetPresignedObjectUrlArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .method(Method.POST)
                    .expiry(expiryMinutes, TimeUnit.MINUTES)
                    .extraQueryParams(formData)
                    .build());
            
            Map<String, String> result = new HashMap<>();
            result.put("url", url);
            result.putAll(formData);
            return result;
        } catch (Exception e) {
            log.error("Failed to generate presigned upload url", e);
            throw new RuntimeException("生成上传链接失败", e);
        }
    }
    
    public List<FileInfo> listFiles(String prefix) {
        List<FileInfo> fileInfos = new ArrayList<>();
        try {
            Iterable<Result<Item>> results = minioClient.listObjects(ListObjectsArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .prefix(prefix)
                    .recursive(true)
                    .build());
            
            for (Result<Item> result : results) {
                Item item = result.get();
                FileInfo fileInfo = new FileInfo();
                fileInfo.setFilename(item.objectName());
                fileInfo.setSize(item.size());
                fileInfo.setLastModified(item.lastModified());
                fileInfo.setContentType(item.contentType());
                fileInfos.add(fileInfo);
            }
        } catch (Exception e) {
            log.error("Failed to list files", e);
            throw new RuntimeException("列出文件失败", e);
        }
        return fileInfos;
    }
    
    public FileInfo getFileInfo(String filename) {
        try {
            StatObjectResponse stat = minioClient.statObject(StatObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(filename)
                    .build());
            
            FileInfo fileInfo = new FileInfo();
            fileInfo.setFilename(filename);
            fileInfo.setSize(stat.size());
            fileInfo.setLastModified(stat.lastModified());
            fileInfo.setContentType(stat.contentType());
            fileInfo.setEtag(stat.etag());
            return fileInfo;
        } catch (Exception e) {
            log.error("Failed to get file info", e);
            throw new RuntimeException("获取文件信息失败", e);
        }
    }
    
    public void copyFile(String sourceFilename, String targetFilename) {
        try {
            minioClient.copyObject(CopyObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .object(targetFilename)
                    .source(CopySource.builder()
                            .bucket(minioProperties.getBucketName())
                            .object(sourceFilename)
                            .build())
                    .build());
            log.info("File copied: {} -> {}", sourceFilename, targetFilename);
        } catch (Exception e) {
            log.error("Failed to copy file", e);
            throw new RuntimeException("文件复制失败", e);
        }
    }
    
    private String generateFilename(String extension) {
        return UUID.randomUUID().toString() + (extension.isEmpty() ? "" : "." + extension);
    }
    
    private String getExtension(String filename) {
        if (filename == null || !filename.contains(".")) {
            return "";
        }
        return filename.substring(filename.lastIndexOf(".") + 1);
    }
}

@Data
public class FileInfo {
    private String filename;
    private Long size;
    private ZonedDateTime lastModified;
    private String contentType;
    private String etag;
}

@RestController
@RequestMapping("/api/files")
@RequiredArgsConstructor
public class FileController {
    
    private final FileStorageService fileStorageService;
    
    @PostMapping("/upload")
    public Result<String> upload(@RequestParam("file") MultipartFile file) {
        String filename = fileStorageService.uploadFile(file);
        return Result.success(filename);
    }
    
    @PostMapping("/upload/path")
    public Result<String> uploadWithPath(
            @RequestParam("file") MultipartFile file,
            @RequestParam("path") String path) {
        String filename = fileStorageService.uploadFileWithPath(file, path);
        return Result.success(filename);
    }
    
    @PostMapping("/upload/batch")
    public Result<List<String>> batchUpload(@RequestParam("files") List<MultipartFile> files) {
        List<String> filenames = fileStorageService.batchUploadFiles(files);
        return Result.success(filenames);
    }
    
    @GetMapping("/download/{filename}")
    public void download(@PathVariable String filename, HttpServletResponse response) {
        fileStorageService.downloadFile(filename, response);
    }
    
    @DeleteMapping("/{filename}")
    public Result<Void> delete(@PathVariable String filename) {
        fileStorageService.deleteFile(filename);
        return Result.success();
    }
    
    @DeleteMapping("/batch")
    public Result<Void> batchDelete(@RequestBody List<String> filenames) {
        fileStorageService.batchDeleteFiles(filenames);
        return Result.success();
    }
    
    @GetMapping("/exists/{filename}")
    public Result<Boolean> exists(@PathVariable String filename) {
        boolean exists = fileStorageService.fileExists(filename);
        return Result.success(exists);
    }
    
    @GetMapping("/url/{filename}")
    public Result<String> getUrl(@PathVariable String filename) {
        String url = fileStorageService.getFileUrl(filename);
        return Result.success(url);
    }
    
    @GetMapping("/presigned/download/{filename}")
    public Result<String> getPresignedDownloadUrl(
            @PathVariable String filename,
            @RequestParam(defaultValue = "60") int expiryMinutes) {
        String url = fileStorageService.getPresignedDownloadUrl(filename, expiryMinutes);
        return Result.success(url);
    }
    
    @GetMapping("/presigned/upload/{filename}")
    public Result<Map<String, String>> getPresignedUploadUrl(
            @PathVariable String filename,
            @RequestParam(defaultValue = "60") int expiryMinutes) {
        Map<String, String> result = fileStorageService.getPresignedUploadUrl(filename, expiryMinutes);
        return Result.success(result);
    }
    
    @GetMapping("/list")
    public Result<List<FileInfo>> list(@RequestParam(required = false) String prefix) {
        List<FileInfo> files = fileStorageService.listFiles(prefix != null ? prefix : "");
        return Result.success(files);
    }
    
    @GetMapping("/info/{filename}")
    public Result<FileInfo> getInfo(@PathVariable String filename) {
        FileInfo fileInfo = fileStorageService.getFileInfo(filename);
        return Result.success(fileInfo);
    }
    
    @PostMapping("/copy")
    public Result<Void> copy(
            @RequestParam("source") String source,
            @RequestParam("target") String target) {
        fileStorageService.copyFile(source, target);
        return Result.success();
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 部署注意事项
1. **数据持久化**：生产环境必须配置数据持久化
2. **访问密钥**：使用强密码，定期更换访问密钥
3. **网络配置**：合理配置网络策略，限制访问来源
4. **监控告警**：配置监控和告警，及时发现问题
5. **备份策略**：制定数据备份策略，定期备份重要数据

### 存储桶管理最佳实践
1. **命名规范**：存储桶名称遵循 DNS 命名规范
2. **访问策略**：遵循最小权限原则设置存储桶策略
3. **版本控制**：重要数据开启版本控制
4. **生命周期**：配置生命周期规则自动管理数据
5. **标签管理**：使用标签分类管理存储桶

### 对象管理注意事项
1. **命名规范**：对象名称避免使用特殊字符
2. **大小限制**：大文件使用分片上传
3. **元数据**：合理使用元数据存储对象信息
4. **内容类型**：正确设置 Content-Type
5. **并发控制**：高并发场景注意对象锁定

### 性能优化
1. **分片上传**：大文件使用分片上传提高效率
2. **预签名 URL**：客户端直接上传下载减轻服务端压力
3. **CDN 加速**：静态资源使用 CDN 加速
4. **连接池**：配置合适的连接池参数
5. **缓存策略**：合理使用缓存减少重复请求

### 安全最佳实践
1. **访问控制**：使用 IAM 策略精细控制访问权限
2. **传输加密**：使用 HTTPS 加密数据传输
3. **静态加密**：敏感数据开启服务端加密
4. **访问日志**：开启访问日志便于审计
5. **定期审计**：定期审计访问日志和权限配置

### SDK 使用注意事项
1. **异常处理**：正确处理各种异常情况
2. **资源释放**：及时释放 InputStream 等资源
3. **重试机制**：网络异常时实现重试
4. **超时配置**：合理设置超时时间
5. **连接管理**：复用 MinioClient 连接

### 最佳实践总结
1. **文件命名**：使用 UUID 或时间戳避免文件名冲突
2. **路径组织**：按业务或日期组织文件路径
3. **元数据利用**：利用元数据存储文件属性
4. **预签名 URL**：客户端直传直传减少服务端负载
5. **CDN 集成**：静态资源通过 CDN 分发
6. **监控告警**：监控存储使用、请求量、错误率
7. **容量规划**：提前规划存储容量，避免空间不足
8. **备份恢复**：定期备份重要数据，演练恢复流程
9. **权限管理**：遵循最小权限原则，定期审计权限
10. **文档维护**：保持使用文档和架构文档更新

## 6. 官方文档参考链接

- MinIO 官方文档：https://min.io/docs
- MinIO Java SDK：https://min.io/docs/minio/linux/developers/java/API.html
- MinIO Python SDK：https://min.io/docs/minio/linux/developers/python/API.html
- MinIO GitHub：https://github.com/minio/minio
- MinIO 中文文档：https://docs.min.io/cn/
