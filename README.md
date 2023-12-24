#include <iostream>
#include <vector>

class BoolMatrix {
private:
    std::vector<std::vector<bool>> matrix;
    int rows;
    int cols;

public:
    
    BoolMatrix(int n, int m) : rows(n), cols(m) {
        matrix.resize(rows, std::vector<bool>(cols, false));
    }

    
    BoolMatrix(const BoolMatrix& other) {
        rows = other.rows;
        cols = other.cols;
        matrix = other.matrix;
    }

    
    std::pair<int, int> size() const {
        return std::make_pair(rows, cols);
    }

    
    BoolMatrix operator||(const BoolMatrix& other) const {
        BoolMatrix result(rows, cols);
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                result.matrix[i][j] = matrix[i][j] || other.matrix[i][j];
            }
        }
        return result;
    }

    
    BoolMatrix operator*(const BoolMatrix& other) const {
        BoolMatrix result(rows, other.cols);
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < other.cols; ++j) {
                for (int k = 0; k < cols; ++k) {
                    result.matrix[i][j] = result.matrix[i][j] || (matrix[i][k] && other.matrix[k][j]);
                }
            }
        }
        return result;
    }

   
    BoolMatrix operator~() const {
        BoolMatrix result(rows, cols);
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                result.matrix[i][j] = !matrix[i][j];
            }
        }
        return result;
    }

    
    int countOnes() const {
        int count = 0;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (matrix[i][j]) {
                    count++;
                }
            }
        }
        return count;
    }

    
    void lexicalSort() {
        for (int i = 0; i < rows - 1; ++i) {
            for (int j = i + 1; j < rows; ++j) {
                if (matrix[i] > matrix[j]) {
                    std::swap(matrix[i], matrix[j]);
                }
            }
        }
    }

    
    BoolMatrix& operator=(const BoolMatrix& other) {
        if (this != &other) {
            rows = other.rows;
            cols = other.cols;
            matrix = other.matrix;
        }
        return *this;
    }

    
    void displayMatrix() const {
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                std::cout << matrix[i][j] << " ";
            }
            std::cout << std::endl;
        }
    }
};

int main() {
    BoolMatrix A(3, 3);
    BoolMatrix B(3, 3);

    
    A = A || B;

    std::cout << "Result Matrix A:" << std::endl;
    A.displayMatrix();

    return 0;
}
