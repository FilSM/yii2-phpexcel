<?php

namespace moonland\phpexcel;

class ExcelFKB289 extends Excel
{
    public $setTitleRow;
    public $setFooterRow;

    /**
     * Setting data from models
     */
    public function executeColumns(&$activeSheet = null, $models, $columns = [], $headers = [])
    {
        if ($activeSheet == null) {
            $activeSheet = $this->activeSheet;
        }
        $hasHeader = false;

        if (isset($this->setTitleRow)) {
            $row = 3;
            $activeSheet->mergeCells('A1:U1');
            $activeSheet->setCellValue('A1', $this->setTitleRow);
        } else {
            $row = 1;
        }

        $char = 26;
        foreach ($models as $model) {
            if (empty($columns)) {
                $columns = $model->attributes();
            }
            if ($this->setFirstTitle && !$hasHeader) {
                $isPlus = false;
                $colplus = 0;
                $colnum = 1;
                foreach ($columns as $key => $column) {
                    $col = '';
                    if ($colnum > $char) {
                        $colplus += 1;
                        $colnum = 1;
                        $isPlus = true;
                    }
                    if ($isPlus) {
                        $col .= chr(64 + $colplus);
                    }
                    $col .= chr(64 + $colnum);
                    $header = '';
                    if (is_array($column)) {
                        if (isset($column['header'])) {
                            $header = $column['header'];
                        } elseif (isset($column['attribute']) && isset($headers[$column['attribute']])) {
                            $header = $headers[$column['attribute']];
                        } elseif (isset($column['attribute'])) {
                            $header = $model->getAttributeLabel($column['attribute']);
                        } elseif (isset($column['cellFormat']) && is_array($column['cellFormat'])) {
                            $activeSheet->getStyle($col . $row)->applyFromArray($column['cellFormat']);
                        }
                    } else {
                        $header = $model->getAttributeLabel($column);
                    }
                    $activeSheet->setCellValue($col . $row, $header);
                    $colnum++;
                }
                $hasHeader = true;
                $row++;
            }
            $isPlus = false;
            $colplus = 0;
            $colnum = 1;
            foreach ($columns as $key => $column) {
                $col = '';
                if ($colnum > $char) {
                    $colplus++;
                    $colnum = 1;
                    $isPlus = true;
                }
                if ($isPlus) {
                    $col .= chr(64 + $colplus);
                }
                $col .= chr(64 + $colnum);
                if (is_array($column)) {
                    $column_value = $this->executeGetColumnData($model, $column);
                    if (isset($column['cellFormat']) && is_array($column['cellFormat'])) {
                        $activeSheet->getStyle($col . $row)->applyFromArray($column['cellFormat']);
                    }
                } else {
                    $column_value = $this->executeGetColumnData($model, ['attribute' => $column]);
                }
                $activeSheet->setCellValue($col . $row, $column_value);
                $colnum++;
            }
            $row++;

            if ($this->autoSize) {
                foreach (range(0, $colnum) as $col) {
                    $activeSheet->getColumnDimensionByColumn($col)->setAutoSize(true);
                }
            }
        }
        if ($this->setFooterRow) {
            $activeSheet->mergeCells('A' . ($row + 1) . ':U' . ($row + 1));
            $activeSheet->setCellValue('A' . ($row + 1), $this->setFooterRow);
        }
    }
}
