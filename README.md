const exportToExcel = () => {
    const worksheet = XLSX.utils.json_to_sheet([]);
    const workbook = XLSX.utils.book_new();

    // Add data from API 1 with column names
    XLSX.utils.sheet_add_json(worksheet, data1, { origin: 'A1' });

    // Add an empty row for spacing
    XLSX.utils.sheet_add_json(worksheet, [{}], { origin: 'A' + (data1.length + 2) });

    // Add data from API 2 with column names
    XLSX.utils.sheet_add_json(worksheet, data2, { origin: 'A' + (data1.length + 3) });

    const worksheetName = 'Combined Data';
    XLSX.utils.book_append_sheet(workbook, worksheet, worksheetName);

    XLSX.writeFile(workbook, 'combined_data.xlsx');
  };


import javax.persistence.EntityManager;
import javax.persistence.TypedQuery;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executor;
import java.util.concurrent.Executors;

public class ParallelJpaPagingAPI {
    private final EntityManager entityManager; // Inject or create an EntityManager
    private final Executor executor = Executors.newFixedThreadPool(5); // 5 threads for parallel execution

    public ParallelJpaPagingAPI(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    public List<YourEntity> getPagesInParallel(int totalPages, int pageSize) throws ExecutionException, InterruptedException {
        List<CompletableFuture<List<YourEntity>>> futures = new ArrayList<>();

        // Divide the total number of pages into 5 parts
        int pagesPerTask = totalPages / 5;

        // Submit tasks for parallel execution
        for (int i = 0; i < 5; i++) {
            int startPage = i * pagesPerTask + 1;
            int endPage = (i == 4) ? totalPages : startPage + pagesPerTask - 1;
            CompletableFuture<List<YourEntity>> future = CompletableFuture.supplyAsync(() -> {
                return queryPages(startPage, endPage, pageSize);
            }, executor);
            futures.add(future);
        }

        // Combine results from all tasks
        List<YourEntity> result = new ArrayList<>();
        for (CompletableFuture<List<YourEntity>> future : futures) {
            result.addAll(future.get());
        }

        return result;
    }

    private List<YourEntity> queryPages(int startPage, int endPage, int pageSize) {
        List<YourEntity> resultList = new ArrayList<>();

        for (int pageNumber = startPage; pageNumber <= endPage; pageNumber++) {
            int startPosition = (pageNumber - 1) * pageSize;
            TypedQuery<YourEntity> query = entityManager.createQuery(
                "SELECT e FROM YourEntity e", YourEntity.class);
            query.setFirstResult(startPosition);
            query.setMaxResults(pageSize);
            resultList.addAll(query.getResultList());
        }

        return resultList;
    }
}
