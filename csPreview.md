# TFrameworknamespace TFramework.Editor
{
    using System.Text;

    public class ScriptsBuildHelp  
    {
        private StringBuilder _Content;
        private string _lineBrake="\r\n";
        private int currentIndex = 0;
        public int IndentTimes { get; set; }

        public ScriptsBuildHelp()
        {
            _Content = new StringBuilder();
        }

        public void Write(string context,bool needIndent=false)
        {
            if (needIndent)
            {
                context = GetIndent() + context;
            }

            if (currentIndex==_Content.Length)
            {
                _Content.Append(context);
            }
            else
            {
                _Content.Insert(currentIndex,context);
            }

            currentIndex += context.Length;
        }

        public void WirteLine(string context,bool needIndex=false)
        {
            Write(context + _lineBrake, needIndex);
        }

        public string GetIndent()
        {
            string indent ="";
            for (int i = 0; i < IndentTimes; i++)
            {
                indent += "   ";
            }
            return indent;
        }

        public int WriteCurlyBrackets()
        {
            var start =_lineBrake+ GetIndent() + "{" + _lineBrake;
            var end = GetIndent() + "}" + _lineBrake;
            Write(start + end, true);
            return end.Length;
        }

        public void WriteUsing(string namespaceName)
        {
            WirteLine("using " + namespaceName + ";");
        }


        public void WriteNamespance(string name)
        {
            Write(_lineBrake+"namespace " + name);
            int length= WriteCurlyBrackets();
            currentIndex -= length;
        }

        public void WriteClass(string name,string baseName=null)
        {
            string tempBase = "";
            if (baseName!=null&&baseName!=string.Empty)
            {
                tempBase = ":" + baseName;
            }
            Write("public class " + name+tempBase,true);
            int length = WriteCurlyBrackets();
            currentIndex -= length;
        }

        public void WriteFun(string name,params string[] paraName)
        {
            StringBuilder temp = new StringBuilder();
            temp.Append("public void " + name + "()");
            if (paraName.Length>0)
            {
                foreach (string s in paraName)
                {
                    temp.Insert(temp.Length - 1, s + ",");
                }
                temp.Remove(temp.Length - 2, 1);
            }
            Write(temp.ToString(), true);
            WriteCurlyBrackets();
        }

        public override string ToString()
        {
            return _Content.ToString();
        }
    }
}

