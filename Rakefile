require "rake/clean"

Charts = ["piechart", "barchart"]
Filetypes = ["svg", "png", "pdf"]

task :default => "tmp/analytical-paragraph.pdf"

directory "tmp"
CLOBBER.include("tmp")

file "tmp/analytical-paragraph.pdf" => ["analytical-paragraph.md", "tmp/barchart.pdf"] do
  sh "pandoc -f markdown -t pdf -o tmp/analytical-paragraph.pdf analytical-paragraph.md"
end

CLOBBER.include("tmp/analytical-paragraph.pdf")

Charts.each do |chart|
  multitask chart => Filetypes.map { |filetype| "tmp/#{chart}.#{filetype}"}

  file "tmp/#{chart}.vg.json" => ["#{chart}.json", "tmp"] do
    sh "npx vl2vg #{chart}.json > tmp/#{chart}.vg.json"
  end

  Filetypes.each do |filetype|
    file "tmp/#{chart}.#{filetype}" => "tmp/#{chart}.vg.json" do
      sh "npx vg2#{filetype} tmp/#{chart}.vg.json tmp/#{chart}.#{filetype}"
    end

    CLOBBER.include("tmp/#{chart}.#{filetype}")
  end
end
