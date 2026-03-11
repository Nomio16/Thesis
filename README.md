# LaTeX Thesis Project Setup

Энэхүү LaTeX төслийг хэрхэн хөрвүүлэх (compile) болон засварлах талаарх заавар:

## 1. Шаардлагатай программ суулгах
Монгол хэл (кирилл үсэг) болон \`Liberation Serif\` фонт (Times New Roman-тай ижил) ашиглаж байгаа тул энгийн \`pdflatex\`-ийн оронд **\`xelatex\`** ашиглах хэрэгтэй.

Убунту / Линукс дээр дараах комманд ашиглаж LaTeX болон фонтыг суулгана:
\`\`\`bash
sudo apt update
sudo apt install texlive-full texlive-xetex texlive-lang-cyrillic fonts-liberation
\`\`\`

## 2. Төслийг хөрвүүлэх (Compile to PDF)
Төслийн хавтаст (\`/home/nomio/Documents/БСА/thesis\`) орж терминал дээр дараах коммандыг ажиллуулснаар \`main.pdf\` үүснэ:

\`\`\`bash
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
\`\`\`
*(Олон удаа ажиллуулдаг шалтгаан нь Гарчиг (Table of Contents) болон Ном зүйг (Bibliography) зөв таниулахын тулд юм.)*

**Эсвэл \`latexmk\` ашиглах нь илүү хялбар байдаг:**
\`\`\`bash
latexmk -xelatex main.tex
\`\`\`

## 3. Файлуудыг засварлах (Editing)
Та ямар ч текст засварлагч (VSCode, TeXstudio, Overleaf) ашиглаж болно.
- **Нүүр болон ерөнхий тохиргоо:** \`main.tex\` файлыг засна.
- **Удиртгал, Нэр томьёо, Товчилсон үгс:** \`chapters/0_frontmatter.tex\` дотор засна.
- **Онолын судалгаа (Бүлэг 1):** \`chapters/1_theoretical_background.tex\`
- **Системийн шинжилгээ ба зохиомж (Бүлэг 2):** \`chapters/2_system_analysis_and_design.tex\`
- **Хэрэгжүүлэлт ба туршилт (Бүлэг 3):** \`chapters/3_implementation_and_testing.tex\`
- **Дүгнэлт:** \`chapters/4_conclusion.tex\`
- **Ном зүй (Ашигласан материал):** \`references.bib\` дотор \`BibTeX\` форматаар нэмнэ.

## 4. Git ашиглаж өөрчлөлт хадгалах
Өөрчлөлт хийсний дараа Git дээр дараах байдлаар хадгалан (commit) цааш Github руугаа push хийх боломжтой:
\`\`\`bash
git add .
git commit -m "Өөрчлөлтийн утга..."
git push origin master  # Хэрвээ Github холбогдсон бол
\`\`\`
