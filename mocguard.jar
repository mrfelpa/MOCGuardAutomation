import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.*;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.persistence.Query;
import java.util.List;
import java.util.logging.Level;
import java.util.logging.Logger;

@Service
public class MOCGuardService {

    @PersistenceContext
    private EntityManager entityManager;

    private static final Logger logger = Logger.getLogger(MOCGuardService.class.getName());

    public void checkMOCVulnerabilities() {
        try {
            List<String> userTables = getUserTables();
            for (String table : userTables) {
                analyzeTable(table);
            }
        } catch (Exception e) {
            logger.log(Level.SEVERE, "Error during MOC vulnerability check: ", e);
        }
    }

    private List<String> getUserTables() {
        // Identify tables that store user data
        Query query = entityManager.createNativeQuery("SELECT table_name FROM information_schema.tables WHERE table_schema = 'public'");
        return query.getResultList();
    }

    private void analyzeTable(String tableName) {
        // Check if ownership checks are present
        try {
            Query query = entityManager.createNativeQuery("SELECT * FROM " + tableName);
            List<Object[]> results = query.getResultList();
            
            for (Object[] row : results) {
                if (!isOwnerCheckValid(row)) {
                    logger.warning("MOC vulnerability detected in table: " + tableName);
                }
            }
        } catch (Exception e) {
            logger.log(Level.SEVERE, "Error analyzing table: " + tableName, e);
        }
    }

    private boolean isOwnerCheckValid(Object[] row) {
        // Logic to verify if the ownership check is correct
        try {
            Integer userId = (Integer) row[0]; // Assuming user ID is in the first column
            Integer ownerId = (Integer) row[1]; // Assuming owner ID is in the second column

            return userId.equals(ownerId); // Return true if ownership check is valid
        } catch (ArrayIndexOutOfBoundsException | ClassCastException e) {
            logger.log(Level.SEVERE, "Error validating owner check: ", e);
            return false; // Return false if there's an error in validation
        }
    }
}

@RestController
@RequestMapping("/api/mocguard")
public class MOCGuardController {

    @Autowired
    private MOCGuardService mocGuardService;

    @GetMapping("/check")
    public String checkForMOCVulnerabilities() {
        mocGuardService.checkMOCVulnerabilities();
        return "Vulnerability check completed.";
    }
}
